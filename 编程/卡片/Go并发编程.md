---
tags:
  - go
  - 并发
up:
  - "[[Go]]"
down:
  - "[[goroutine]]"
  - "[[channel]]"
  - "[[select]]"
  - "[[context]]"
  - "[[sync包]]"
  - "[[Go sync-atomic]]"
  - "[[Go内存模型]]"
  - "[[Go channel模式]]"
relation:
  - "[[协程]]"
  - "[[用户级线程与内核级线程]]"
  - "[[同步和锁]]"
  - "[[阻塞、非阻塞与异步、同步]]"
---

# Go并发编程

Go 的并发模型以 goroutine 为执行单元，以 channel、context 和 sync 包完成通信、取消和共享内存同步。

1. [[goroutine]]
2. [[channel]]
3. [[select]]
4. [[context]]
5. [[sync包]]
6. [[Go sync-atomic]]
7. [[Go内存模型]]
8. [[Go channel模式]]

## 常见原则

1. 不要通过共享内存来通信，而要通过通信来共享内存。
2. channel 适合表达所有权转移、任务队列和事件通知。
3. mutex 适合保护共享状态，尤其是临界区小、数据结构明确的场景。
4. context 负责取消、超时和请求级元数据传递。
