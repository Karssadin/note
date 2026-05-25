---
tags:
  - go
  - 并发
up:
  - "[[Go并发编程]]"
down:
relation:
  - "[[阻塞、非阻塞与异步、同步]]"
  - "[[select]]"
  - "[[Go并发编程]]"
---

# channel

channel 是 Go 中 goroutine 之间通信和同步的基础设施。

```go
ch := make(chan int)
ch <- 1
v := <-ch
```

## 分类

1. 无缓冲 channel：发送和接收必须同时准备好，天然同步。
2. 有缓冲 channel：缓冲区未满可发送，非空可接收。
3. 单向 channel：限制只发送或只接收，常用于函数签名表达约束。

## 关闭

1. 发送方负责关闭 channel。
2. 向已关闭 channel 发送会 panic。
3. 从已关闭 channel 接收会得到零值和 `ok=false`。
4. 不要通过关闭 channel 通知多个发送方停止发送，通常应结合 context。
