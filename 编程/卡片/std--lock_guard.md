---
tags: 
up: 
  - "[[C++]]"
down: 
relation:
  - "[[std--unique_lock]]"
---
  

- 一个互斥量包装程序，它提供了一种方便的RAII（Resource acquisition is initialization ）风格的机制来在作用域块的持续时间内拥有一个互斥量。
- 当创建一个 lock_guard 对象时，它会自动获取给定的互斥锁，并在其生命周期结束时释放该互斥锁。
- 使用 lock_guard 可以简化锁管理，减少由于异常或遗漏 unlock 调用而导致的错误。
- 特点如下：
    - 创建即加锁，作用域结束自动析构并解锁，无需手工解锁
    - 不能中途解锁，必须等作用域结束才解锁
    - 不能复制
    - 无需手动调用lock和unlock，在构造时候lock，析构时解锁

```C
\#include <mutex>
\#include <thread>

std::mutex mtx;

void print_function() {
    std::lock_guard<std::mutex> lock(mtx);  // 自动锁定互斥量
    std::cout << "Hello from thread " << std::this_thread::get_id() << std::endl;
    // 在函数或作用域结束时，析构函数自动解锁
}

void print_function() {
    mtx.lock();  // 手动锁定互斥量
    std::cout << "Hello from thread " << std::this_thread::get_id() << std::endl;
    mtx.unlock();  // 手动解锁互斥量
}
```