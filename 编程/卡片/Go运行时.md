---
tags:
  - go
  - 运行时
up:
  - "[[Go]]"
down:
  - "[[Go GMP调度模型]]"
  - "[[Go netpoll]]"
  - "[[Go defer-panic-recover]]"
  - "[[Go reflect]]"
  - "[[Go unsafe]]"
relation:
  - "[[用户级线程与内核级线程]]"
  - "[[协程]]"
  - "[[IO多路复用及其技术（select、poll、epoll）]]"
---

# Go运行时

Go runtime 负责 goroutine 调度、栈管理、垃圾回收、网络轮询、defer/panic/recover 等语言运行期能力。

1. [[Go GMP调度模型]]
2. [[Go netpoll]]
3. [[Go defer-panic-recover]]
4. 栈增长
	1. goroutine 栈从小栈开始
	2. runtime 在需要时扩容
5. GC 协作
	1. 写屏障维护并发标记正确性
	2. STW 只保留在必要阶段
