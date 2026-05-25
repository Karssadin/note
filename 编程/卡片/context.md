---
tags:
  - go
  - 并发
up:
  - "[[Go并发编程]]"
down:
relation:
  - "[[goroutine]]"
  - "[[channel]]"
  - "[[HTTP请求包含哪些部分]]"
---

# context

`context.Context` 用于在 API 边界上传递取消信号、超时截止时间和请求级值。

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
defer cancel()
```

## 作用

1. 取消 goroutine。
2. 控制超时和截止时间。
3. 在请求链路中传递少量元数据。

## 使用原则

1. 函数第一个参数通常是 `ctx context.Context`。
2. 不要把 context 存进结构体作为长期状态。
3. `WithCancel`、`WithTimeout`、`WithDeadline` 返回的 cancel 应及时调用。
4. context value 只适合请求级数据，不适合传普通业务参数。
