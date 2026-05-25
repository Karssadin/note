---
tags:
  - go
  - 并发
up:
  - "[[Go与其他语言对比]]"
down:
relation:
  - "[[Go并发编程]]"
  - "[[用户级线程与内核级线程]]"
  - "[[协程]]"
---

# Go并发模型对比

Go 的并发模型把 goroutine 作为轻量执行单元，由 runtime 映射到 OS 线程执行。

| 模型 | 代表 | 特点 |
|------|------|------|
| OS 线程 | C++ pthread、Java Thread | 内核调度，资源开销较大 |
| 事件循环 | Node.js、Redis | 单线程事件驱动，适合 IO |
| 协程 | Python asyncio、C++20 coroutine | 用户态挂起恢复，需要调度器配合 |
| goroutine | Go | M:N 调度，语言和 runtime 原生支持 |

## Go 的特点

1. goroutine 写法接近同步代码。
2. channel 可表达通信和同步。
3. runtime 负责调度、栈增长和网络轮询。
4. 共享内存仍需锁或原子操作保护。
