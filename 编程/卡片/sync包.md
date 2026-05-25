---
tags:
  - go
  - 并发
up:
  - "[[Go并发编程]]"
down:
  - "[[Go sync-atomic]]"
relation:
  - "[[同步和锁]]"
  - "[[互斥锁]]"
  - "[[读写锁]]"
---

# sync包

`sync` 包提供互斥锁、读写锁、条件变量、一次执行、等待组和并发安全 Map 等同步工具。

## 常用类型

1. `sync.Mutex`：互斥锁，保护临界区。
2. `sync.RWMutex`：读写锁，适合读多写少。
3. `sync.WaitGroup`：等待一组 goroutine 结束。
4. `sync.Once`：确保函数只执行一次。
5. `sync.Cond`：条件变量。
6. `sync.Map`：特定场景下的并发安全 Map。
7. `sync.Pool`：临时对象复用池。
8. [[Go sync-atomic]]：简单共享状态的原子操作。

## 注意

1. 锁不要复制，包含锁的结构体也应避免复制。
2. `WaitGroup.Add` 应在启动 goroutine 前调用。
3. `sync.Map` 不是普通 map 的无脑替代。
4. 复杂不变量优先用锁保护，不要为了无锁而滥用 atomic。
