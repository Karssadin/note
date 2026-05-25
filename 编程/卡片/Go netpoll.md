---
tags:
  - go
  - 网络
  - 运行时
up:
  - "[[Go运行时]]"
down:
relation:
  - "[[IO多路复用及其技术（select、poll、epoll）]]"
  - "[[网络模型]]"
  - "[[Go net-http包]]"
---

# Go netpoll

Go runtime 的 netpoll 用于把网络 IO 等待接入调度器，避免 goroutine 因等待网络事件而长期占用 OS 线程。

## 基本思路

1. 网络 fd 通常设置为非阻塞。
2. goroutine 遇到未就绪 IO 时挂起。
3. runtime 把 fd 注册到平台事件机制中。
4. 事件就绪后，netpoll 唤醒对应 goroutine。

## 平台实现

1. Linux：epoll
2. macOS / BSD：kqueue
3. Windows：IOCP

## 意义

1. 大量连接可以由少量线程承载。
2. 与 GMP 调度结合，屏蔽底层事件机制复杂度。
3. 让网络编程保持同步代码风格。
