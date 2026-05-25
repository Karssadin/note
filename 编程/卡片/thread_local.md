---
tags:
  - C++
up:
  - "[[C++]]"
down:
relation:
  - "[[std--thread]]"
  - "[[静态变量：static]]"
---

C++11 引入 `thread_local` 存储类修饰符，声明的变量在每个线程中拥有独立的副本，线程结束时自动销毁。

## 基本用法

```cpp
thread_local int counter = 0;

void work() {
    counter++;  // 每个线程独立计数
    std::cout << "thread " << std::this_thread::get_id()
              << ": counter = " << counter << "\n";
}

int main() {
    std::thread t1(work);  // counter = 1
    std::thread t2(work);  // counter = 1（独立副本）
    t1.join(); t2.join();
}
```

## 与其他存储期对比

| 修饰符 | 生命周期 | 每线程独立 |
|--------|---------|-----------|
| 局部变量 | 函数调用期间 | 天然独立（在栈上） |
| `static` | 程序全程 | 所有线程共享 |
| `thread_local` | 线程全程 | 每线程一份 |
| 动态分配 | 手动控制 | 取决于指针归属 |

## 适用场景

1. **线程安全的全局状态**：errno、日志上下文、随机数生成器种子
2. **避免加锁**：每个线程有自己的缓存/缓冲区，无需互斥
3. **性能计数器**：各线程独立累计，最后汇总

## 注意事项

- `thread_local` 变量的构造在首次使用时（而非线程创建时），析构在线程退出时
- 可以与 `static` 联合使用：`static thread_local int x;`（类中使用时必须加 `static`）
- 动态库中的 `thread_local` 变量在某些平台上有性能开销（TLS 需要间接寻址）
