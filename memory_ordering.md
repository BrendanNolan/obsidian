# Release and Acquire

## memory_order_release

`memory_order_release` prevents operations **before** it from being reordered **after** it:

```cpp
buffer_[i] = value;                              // Cannot move after this line
head_.store(i, std::memory_order_release);       // Release barrier
z = 3;                                           // CAN move before the barrier
```

More specifically:

- All memory operations before the release are guaranteed to complete before the release store
  executes
- When another thread **acquires** the same atomic variable, they synchronize with this release
- The guarantee is **pairwise**: only threads that acquire the variable see the prior operations
- It does NOT make everything immediately visible to all threads—it's a synchronization point

## memory_order_acquire

`memory_order_acquire` prevents operations **after** it from being reordered **before** it:

```cpp
auto idx = head_.load(std::memory_order_acquire);  // Acquire barrier
value = buffer_[idx];                              // Cannot move before this line
z = 3;                                             // CAN move after the barrier
```

More specifically:

- All memory operations after the acquire are guaranteed to not execute before the acquire load
- It synchronizes with a prior `memory_order_release` on the same atomic variable
- The acquiring thread sees all operations that the releasing thread made before its release
- The guarantee is **pairwise**: only synchronizes with releases on this specific variable

## Directionality of Visibility

Release and acquire form a one-way channel between threads. When thread `A` does a release store and
thread `B` subsequently does an acquire load of the same variable (seeing `A` 's stored value),
everything `A` wrote _before_ its release is visible to everything `B` reads _after_ its acquire.

Critically, this only flows in one direction:

- **Release** is a one-way barrier that "pushes" all preceding writes out before the store. Writes
  _after_ the release have no guarantee.
- **Acquire** is a one-way barrier that "holds back" all subsequent reads until after the load.
  Reads _before_ the acquire have no guarantee.

This is the "happens-before" edge: it runs from the release to the acquire, never the reverse.

## Release-Acquire Pairing in Action with SPSC Queue

```cpp
// Producer enqueue
const size_t current_head = head_.load(std::memory_order_relaxed);      // Own index, no sync needed
const size_t next_head = (current_head + 1) & MASK;
if (next_head == tail_.load(std::memory_order_acquire)) {               // Acquire: see consumer's progress
    return std::unexpected(value);
}
buffer_[current_head] = value;
head_.store(next_head, std::memory_order_release);                      // Release: publish to consumer

// Consumer dequeue
const size_t current_tail = tail_.load(std::memory_order_relaxed);      // Own index, no sync needed
if (current_tail == head_.load(std::memory_order_acquire)) {            // Acquire: see producer's progress
    return std::nullopt;
}
auto value = std::move(buffer_[current_tail]);
const size_t next_tail = (current_tail + 1) & MASK;
tail_.store(next_tail, std::memory_order_release);                      // Release: publish to producer
return value;
```

**Rationale**:

- `relaxed` on own index: only this thread writes it
- `acquire` on other thread's index: synchronize with their releases
- `release` on publishing index: ensure our buffer writes are visible

# Full Implementation: SpinLock

A simple spinlock using `exchange()` with acquire-release ordering:

```cpp
class SpinLock {
private:
    std::atomic<bool> locked_{false};
public:
    void lock() {
        while (locked_.exchange(true, std::memory_order_acquire)) {
            // Spin until we acquired the lock
        }
    }
    void unlock() {
        locked_.store(false, std::memory_order_release);
    }
};
```

**How it works**:

- `lock()`: Uses `exchange(true, acquire)` to atomically swap the flag to true and get the old value
  - If it was false, we acquired the lock and exit the loop
  - If true, we keep spinning until we get false
- `unlock()`: Uses `store(false, release)` to release the lock
- Memory ordering ensures any critical section after `lock()` is ordered after the acquisition, and
  any critical section before `unlock()` is ordered before the release

# Full Implementation: SPSC Lock-Free Queue

A single-producer, single-consumer queue using a ring buffer and acquire-release synchronization.
Some things to note:

- `enqueue` acquires the tail and releases the head
- `dequeue` acquires the head and releases the tail

```cpp
template <typename T, size_t Capacity = 255>
class SPSCQueue {
    static constexpr size_t BufSize = Capacity + 1;
    static_assert((BufSize & (BufSize - 1)) == 0, "Capacity + 1 must be a power of 2");
private:
    static constexpr size_t MASK = BufSize - 1;
    alignas(64) std::atomic<size_t> head_{0};  // Producer index
    alignas(64) std::atomic<size_t> tail_{0};  // Consumer index
    alignas(64) std::array<T, BufSize> buffer_;
public:
    SPSCQueue() = default;
    ~SPSCQueue() = default;
    SPSCQueue(const SPSCQueue&) = delete;
    SPSCQueue& operator=(const SPSCQueue&) = delete;
    // Enqueue: only called by producer
    std::expected<void, T> enqueue(T value) {
        const auto current_head = head_.load(std::memory_order_relaxed);
        const auto next_head = (current_head + 1) & MASK;
        const auto full = next_head == tail_.load(std::memory_order_acquire);
        if (full) {
            return std::unexpected(std::move(value));
        }
        // The queue is not full and it will not be made full by the other thread while we work
        // - because the other thread only consumes - so we may freely add our element.
        buffer_[current_head] = std::move(value);
        head_.store(next_head, std::memory_order_release);
        return {};
    }
    // Dequeue: only called by consumer
    std::optional<T> dequeue() {
        const auto current_tail = tail_.load(std::memory_order_relaxed);
        const auto empty = current_tail == head_.load(std::memory_order_acquire);
        if (empty) {
            return std::nullopt;
        }
        // The queue is not empty and it will not be made empty by any other thread while we work,
        // - because the other thread only produces - so we may freely remove our element.
        auto value = std::move(buffer_[current_tail]);
        const auto next_tail = (current_tail + 1) & MASK;
        tail_.store(next_tail, std::memory_order_release);
        return value;
    }
    // These methods are inherently racy (no atomic snapshot of both head_ and tail_),
    // so results may be stale by the time the caller acts on them. Relaxed ordering is
    // sufficient since they are only useful for diagnostics, not correctness decisions.
    bool is_empty() const {
        return tail_.load(std::memory_order_relaxed) ==
               head_.load(std::memory_order_relaxed);
    }
    bool is_full() const {
        const auto next_head = (head_.load(std::memory_order_relaxed) + 1) & MASK;
        return next_head == tail_.load(std::memory_order_relaxed);
    }
    size_t size() const {
        const auto h = head_.load(std::memory_order_relaxed);
        const auto t = tail_.load(std::memory_order_relaxed);
        // Mathematically the distance is (h - t) modulo BufSize, but when we do the computation on
        // a computer in the h < t case, the unsigned integers will wrap around and the copmutation
        // only works because they wrap around modulo a power of 2.
        return (h - t) & MASK;
    }
};
```

**Key design points**:

- **Ring buffer**: Uses modulo arithmetic (`& MASK`) to wrap indices, requiring `Capacity + 1` to be
  a power of 2
- **Cache-line alignment**: `alignas(64)` separates `head_` and `tail_` to prevent false sharing
- **Non-atomic buffer**: The `std::array<T, BufSize>` buffer is protected by the atomic head/tail
  guards
- **Memory ordering**:
  - Producer: relaxed read of own index, acquire read of consumer's index, release write of updated
    index
  - Consumer: relaxed read of own index, acquire read of producer's index, release write of updated
    index
- **One wasted slot**: Queue is full when `(head + 1) & MASK == tail` to distinguish empty from
  full. The extra slot is internal — `Capacity` reflects the true usable capacity
- **Optional return**: `dequeue()` returns `std::optional<T>` for cleaner error handling
