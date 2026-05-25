---
tags:
  - go
  - 并发
up:
  - "[[Go并发编程]]"
down:
relation:
  - "[[channel]]"
  - "[[IO多路复用及其技术（select、poll、epoll）]]"
---

# select

Go 的 `select` 用于在多个 channel 操作之间等待，常用于超时、取消和多路事件处理。

```go
select {
case v := <-ch:
	_ = v
case <-ctx.Done():
	return ctx.Err()
default:
}
```

## 特点

1. 如果多个 case 同时就绪，会伪随机选择一个执行。
2. 没有 case 就绪且没有 default 时会阻塞。
3. default 分支可实现非阻塞发送或接收。
4. `nil` channel 永远不会就绪，可用于动态启停某个 case。

## 常见场景

1. 监听 context 取消。
2. 实现超时控制。
3. 合并多个 channel。
4. 防止 goroutine 永久阻塞。
