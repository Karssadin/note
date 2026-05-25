---
tags:
  - go
  - 并发
up:
  - "[[Go并发编程]]"
down:
relation:
  - "[[sync包]]"
  - "[[互斥锁]]"
  - "[[std--atomic]]"
---

# Go sync-atomic

`sync/atomic` 提供原子读写、加减、比较交换等操作，适合简单共享状态的无锁同步。

## 常用操作

1. `atomic.Load`：原子读取。
2. `atomic.Store`：原子写入。
3. `atomic.Add`：原子加减。
4. `atomic.CompareAndSwap`：比较并交换。
5. `atomic.Value`：保存并原子替换完整值。

## 适用场景

1. 计数器。
2. 状态标记。
3. 读多写少的配置快照。

## 注意

1. 原子操作只适合很小的共享状态。
2. 复杂临界区优先使用 `sync.Mutex`。
3. 不要混用普通读写和原子读写访问同一个变量。
