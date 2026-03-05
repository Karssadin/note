---
tags:
up:
  - "[[C++]]"
down:
relation:
  - "[[互斥锁]]"
  - "[[乱序执行]]"
  - "[[锁与并发控制]]"
---

`std::atomic<T>` 是 C++11 引入的原子类型模板，提供无锁的线程安全操作，避免使用互斥锁带来的开销。

## 基本用法

```cpp
#include <atomic>

std::atomic<int> counter{0};

// 线程安全的自增
counter++;                    // 等价于 fetch_add(1)
counter.fetch_add(1);         // 返回旧值
counter.store(10);            // 原子写
int val = counter.load();     // 原子读
```

## 常用操作

| 操作 | 说明 |
|------|------|
| `load()` | 原子读取 |
| `store(val)` | 原子写入 |
| `exchange(val)` | 原子交换，返回旧值 |
| `compare_exchange_weak/strong(expected, desired)` | CAS 操作 |
| `fetch_add/sub/and/or/xor` | 原子算术/位运算，返回旧值 |

## CAS（Compare-And-Swap）

无锁编程的核心原语：

```cpp
std::atomic<int> val{5};
int expected = 5;
bool ok = val.compare_exchange_strong(expected, 10);
// 如果 val == expected(5)，则将 val 设为 10，返回 true
// 否则 expected 被更新为 val 的当前值，返回 false
```

`weak` 版本在某些平台上可能虚假失败（spurious failure），但循环中性能更优。

## 内存序（Memory Order）

控制原子操作的可见性和排序保证：

| 内存序 | 语义 | 场景 |
|--------|------|------|
| `memory_order_relaxed` | 仅保证原子性，不保证顺序 | 计数器 |
| `memory_order_acquire` | 当前线程后续读写不会重排到此操作前 | 配合 release 用于同步 |
| `memory_order_release` | 当前线程之前的读写不会重排到此操作后 | 配合 acquire 用于同步 |
| `memory_order_acq_rel` | 同时具有 acquire 和 release 语义 | read-modify-write |
| `memory_order_seq_cst` | 全局顺序一致（默认） | 最安全但最慢 |

```cpp
std::atomic<bool> ready{false};
int data = 0;

// 线程 A
data = 42;
ready.store(true, std::memory_order_release);

// 线程 B
while (!ready.load(std::memory_order_acquire)) {}
assert(data == 42);  // 保证看到 data = 42
```

## std::atomic_flag

最轻量的原子类型，仅支持 `test_and_set()` 和 `clear()`，可用于实现自旋锁：

```cpp
std::atomic_flag lock = ATOMIC_FLAG_INIT;

void spin_lock()   { while (lock.test_and_set(std::memory_order_acquire)); }
void spin_unlock() { lock.clear(std::memory_order_release); }
```
