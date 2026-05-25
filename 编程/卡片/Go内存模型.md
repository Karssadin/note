---
tags:
  - go
  - 并发
  - 内存管理
up:
  - "[[Go并发编程]]"
down:
relation:
  - "[[Go内存管理]]"
  - "[[sync包]]"
  - "[[Go sync-atomic]]"
---

# Go内存模型

Go 内存模型描述在并发程序中，一个 goroutine 对变量的写入在什么条件下能被另一个 goroutine 可靠观察到。

## happens-before

如果事件 A happens-before 事件 B，那么 A 的写入对 B 可见。常见同步关系包括：

1. goroutine 创建前的语句 happens-before 新 goroutine 开始执行。
2. channel 发送 happens-before 对应接收完成。
3. close channel happens-before 接收到关闭事件。
4. `Mutex.Unlock` happens-before 后续成功的 `Mutex.Lock`。
5. `sync.Once` 中函数执行完成 happens-before 所有 `Once.Do` 返回。
6. atomic 操作提供相应的同步语义。

## 原则

1. 有数据竞争的程序行为不可靠。
2. 不要依赖 goroutine 调度顺序表达同步。
3. 用 channel、锁、WaitGroup、Once、atomic 等明确建立同步关系。
