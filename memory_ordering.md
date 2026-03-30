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

## Release-Acquire Pairing in Action with SPSC Queue

```cpp
// Producer enqueue
const size_t current_head = head_.load(std::memory_order_relaxed);      // Own index, no sync needed
const size_t next_head = (current_head + 1) & MASK;
if (next_head == tail_.load(std::memory_order_acquire)) {               // Acquire: see consumer's progress
    return false;
}
buffer_[current_head] = value;
head_.store(next_head, std::memory_order_release);                      // Release: publish to consumer

// Consumer dequeue
const size_t current_tail = tail_.load(std::memory_order_relaxed);      // Own index, no sync needed
if (current_tail == head_.load(std::memory_order_acquire)) {            // Acquire: see producer's progress
    return std::nullopt;
}
T value = std::move(buffer_[current_tail]);
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

A single-producer, single-consumer queue using a ring buffer and acquire-release synchronization:

```cpp
template <typename T, size_t Capacity = 256>
class SPSCQueue {
    static_assert((Capacity & (Capacity - 1)) == 0, "Capacity must be a power of 2");
private:
    static constexpr size_t MASK = Capacity - 1;
    alignas(64) std::atomic<size_t> head_{0};  // Producer index
    alignas(64) std::atomic<size_t> tail_{0};  // Consumer index
    alignas(64) std::array<T, Capacity> buffer_;
public:
    SPSCQueue() = default;
    ~SPSCQueue() = default;
    SPSCQueue(const SPSCQueue&) = delete;
    SPSCQueue& operator=(const SPSCQueue&) = delete;
    // Enqueue: only called by producer
    bool enqueue(T value) {
        const size_t current_head = head_.load(std::memory_order_relaxed);
        const size_t next_head = (current_head + 1) & MASK;
        // Check if queue is full
        if (next_head == tail_.load(std::memory_order_acquire)) {
            return false;
        }
        buffer_[current_head] = value;
        head_.store(next_head, std::memory_order_release);
        return true;
    }
    // Dequeue: only called by consumer
    std::optional<T> dequeue() {
        const size_t current_tail = tail_.load(std::memory_order_relaxed);
        // Check if queue is empty
        if (current_tail == head_.load(std::memory_order_acquire)) {
            return std::nullopt;
        }
        T value = std::move(buffer_[current_tail]);
        const size_t next_tail = (current_tail + 1) & MASK;
        tail_.store(next_tail, std::memory_order_release);
        return value;
    }
    bool is_empty() const {
        return tail_.load(std::memory_order_relaxed) ==
               head_.load(std::memory_order_relaxed);
    }
    bool is_full() const {
        const size_t next_head = (head_.load(std::memory_order_relaxed) + 1) & MASK;
        return next_head == tail_.load(std::memory_order_relaxed);
    }
    size_t size() const {
        const size_t h = head_.load(std::memory_order_relaxed);
        const size_t t = tail_.load(std::memory_order_relaxed);
        return (h - t) & MASK;
    }
};
```

**Key design points**:

- **Ring buffer**: Uses modulo arithmetic (`& MASK`) to wrap indices, requiring capacity to be a
  power of 2
- **Cache-line alignment**: `alignas(64)` separates `head_` and `tail_` to prevent false sharing
- **Non-atomic buffer**: The `std::array<T, Capacity>` buffer is protected by the atomic head/tail
  guards
- **Memory ordering**:
  - Producer: relaxed read of own index, acquire read of consumer's index, release write of updated
    index
  - Consumer: relaxed read of own index, acquire read of producer's index, release write of updated
    index
- **One wasted slot**: Queue is full when `(head + 1) & MASK == tail` to distinguish empty from full
- **Optional return**: `dequeue()` returns `std::optional<T>` for cleaner error handling
