# Condition Variables

## How They Work

A condition variable reduces unnecessary thread waking by allowing threads to **sleep until a
specific event occurs**, rather than constantly polling.

**Key benefits:**

- **Correctness**: Atomic check-and-wait closes race conditions
- **Efficiency**: Event-driven instead of polling

## C++ Example

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <queue>

int main() {
    std::queue<int> q;
    std::mutex mtx;
    std::condition_variable cv;

    // Consumer thread
    std::thread consumer([&]() {
        for (int i = 0; i < 5; i++) {
            std::unique_lock<std::mutex> lock(mtx);
            cv.wait(lock, [&]() { return !q.empty(); });  // Wait for queue to have data

            int val = q.front();
            q.pop();
            std::cout << "Consumed: " << val << std::endl;
        }
    });

    // Producer thread
    std::thread producer([&]() {
        for (int i = 0; i < 5; i++) {
            std::this_thread::sleep_for(std::chrono::milliseconds(100));

            {
                std::lock_guard<std::mutex> lock(mtx);
                q.push(i);
                std::cout << "Produced: " << i << std::endl;
            }
            cv.notify_one();  // Wake the consumer
        }
    });

    producer.join();
    consumer.join();
    return 0;
}
```

**Key points:**

- `cv.wait(lock, predicate)` atomically unlocks, sleeps, and re-acquires
- The lambda predicate rechecks the condition on every wakeup (handles spurious wakes)
- `notify_one()` wakes one waiting thread

## Rust Example

```rust
use std::sync::{Mutex, Condvar, Arc};
use std::thread;
use std::time::Duration;
use std::collections::VecDeque;

fn main() {
    let queue = Arc::new(Mutex::new(VecDeque::new()));
    let cv = Arc::new(Condvar::new());

    let queue_consumer = Arc::clone(&queue);
    let cv_consumer = Arc::clone(&cv);

    // Consumer thread
    let consumer = thread::spawn(move || {
        for _ in 0..5 {
            let mut q = queue_consumer.lock().unwrap();
            while q.is_empty() {
                q = cv_consumer.wait(q).unwrap();  // Release lock, sleep, re-acquire
            }
            let val = q.pop_front();
            println!("Consumed: {:?}", val);
        }
    });

    let queue_producer = Arc::clone(&queue);
    let cv_producer = Arc::clone(&cv);

    // Producer thread
    let producer = thread::spawn(move || {
        for i in 0..5 {
            thread::sleep(Duration::from_millis(100));
            {
                let mut q = queue_producer.lock().unwrap();
                q.push_back(i);
                println!("Produced: {}", i);
            }
            cv_producer.notify_one();
        }
    });

    producer.join().unwrap();
    consumer.join().unwrap();
}
```

**Key differences from C++:**

- `Condvar::wait(guard)` takes the lock guard and returns it
- Rust's borrow checker enforces you hold the lock during wait
- The predicate is a `while` loop, not a lambda
- `Arc` shares ownership across threads
- RAII guards handle lock/unlock automatically
