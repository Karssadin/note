---
tags:
up:
  - "[[C++]]"
down:
relation:
  - "[[std--thread]]"
  - "[[lambda]]"
---

C++11 引入了 `std::async`、`std::future`、`std::promise`、`std::packaged_task` 四个异步编程组件，用于在线程间传递计算结果。

## std::async + std::future

最简单的异步方式：启动任务，稍后获取结果。

```cpp
#include <future>

auto future = std::async(std::launch::async, [](int x) {
    return x * x;
}, 10);

// 做其他事情...

int result = future.get();  // 阻塞等待结果，返回 100
```

`std::launch` 策略：
- `std::launch::async`：强制在新线程中执行
- `std::launch::deferred`：延迟执行，调用 `get()` 时才在当前线程执行
- 默认（两者皆可）：由实现决定

## std::promise

手动设置 future 的值，实现线程间单向数据传递：

```cpp
std::promise<int> prom;
std::future<int> fut = prom.get_future();

std::thread t([&prom]() {
    int result = compute();
    prom.set_value(result);     // 设置值
    // prom.set_exception(...)  // 或传递异常
});

int val = fut.get();  // 阻塞等待 promise 被设置
t.join();
```

## std::packaged_task

将可调用对象包装为异步任务，可以在任意时机启动：

```cpp
std::packaged_task<int(int, int)> task([](int a, int b) {
    return a + b;
});

std::future<int> fut = task.get_future();
std::thread t(std::move(task), 3, 4);  // 在新线程中启动

int result = fut.get();  // 7
t.join();
```

## 对比

| 组件 | 控制程度 | 适用场景 |
|------|---------|---------|
| `std::async` | 低（自动管理线程） | 简单的 fire-and-forget 任务 |
| `std::promise` | 中（手动设值） | 线程间传递单次结果 |
| `std::packaged_task` | 高（可存储、延迟启动） | 任务队列、线程池实现 |

## 注意事项

- `future.get()` 只能调用一次，第二次调用行为未定义
- 如需多次读取结果，使用 `std::shared_future`
- `std::async` 返回的 future 析构时会自动等待任务完成（隐式 join）
