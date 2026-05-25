---
tags:
  - go
  - 并发
  - 运行时
up:
  - "[[Go运行时]]"
down:
relation:
  - "[[goroutine]]"
  - "[[用户级线程与内核级线程]]"
  - "[[协程]]"
---

# Go GMP调度模型

GMP 是 Go runtime 的调度模型：G 表示 goroutine，M 表示 OS 线程，P 表示处理器资源和本地运行队列。

## 三个角色

1. G：goroutine，保存栈、指令位置和调度状态。
2. M：machine，实际执行代码的 OS 线程。
3. P：processor，持有本地队列，是运行 Go 代码所需的调度资源。

## 调度过程

1. M 必须绑定 P 才能执行 G。
2. P 优先从本地队列取 G。
3. 本地队列为空时，会从全局队列或其他 P 偷取 G。
4. 网络 IO 可通过 netpoll 唤醒等待的 G。
5. 系统调用阻塞时，runtime 会尽量让 P 脱离阻塞的 M，交给其他 M 继续执行。

## 相关参数

`GOMAXPROCS` 控制同时执行 Go 代码的 P 数量，默认通常等于 CPU 核数。
