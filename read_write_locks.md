# Read-Write Lock Implementation

A basic read-write lock implementation in C++ using `std::mutex` and `std::condition_variable`:

## Implementation

```cpp
#include <mutex>
#include <condition_variable>

class RWLock {
private:
    mutable std::mutex m;
    std::condition_variable cv;
    int readers = 0;
    bool writer = false;

public:
    void lock_read() {
        std::unique_lock lock(m);
        cv.wait(lock, [this] { return !writer; });
        readers++;
    }

    void unlock_read() {
        std::unique_lock lock(m);
        readers--;
        cv.notify_all();
    }

    void lock_write() {
        std::unique_lock lock(m);
        cv.wait(lock, [this] { return !writer && readers == 0; });
        writer = true;
    }

    void unlock_write() {
        std::unique_lock lock(m);
        writer = false;
        cv.notify_all();
    }
};
```

## Usage Example

```cpp
RWLock lock;
int shared_data = 0;

// Reader thread
{
    lock.lock_read();
    int value = shared_data;
    lock.unlock_read();
}

// Writer thread
{
    lock.lock_write();
    shared_data = 42;
    lock.unlock_write();
}
```

## How It Works

- `readers` counts active readers; `writer` tracks if a writer holds the lock
- Readers wait until no writer is active, then increment the reader count
- Writers wait until no readers or writers are active, then set the writer flag
- `notify_all()` wakes up waiting threads when a lock is released
- Multiple readers can proceed simultaneously, but writers get exclusive access
