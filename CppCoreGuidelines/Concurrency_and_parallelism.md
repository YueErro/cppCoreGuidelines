# CP: Concurrency and parallelism
For threaded environment.

## Table of contents
* [CP.free: Lock-free programming](#cpfree-lock-free-programming)
  * [CP.111: Use a convetional pattern if you really need double-checked locking](https://github.com/YueErro/cppCoreGuidelines/blob/master/CppCoreGuidelines/Concurrency_and_parallelism.md#cp111-use-a-convetional-pattern-if-you-really-need-double-checked-locking)

### CP.free: Lock-free programming

#### CP.111: Use a convetional pattern if you really need double-checked locking
```cpp
mutex action_mutex;
atomic<bool> action_needed;   // do
volatile bool action_needed;  // do not

if ( action_needed.load(memory_order_acquire) ){
  lock_guard<std::mutex> lock(action_mutex);
  if ( action_needed.load(memory_order_relaxed) ){
    take_action();
    action_needed.store(false, memory_order_release);
  }
}
```
