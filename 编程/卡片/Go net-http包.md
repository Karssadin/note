---
tags:
  - go
  - 标准库
  - 计算机网络
up:
  - "[[Go标准库]]"
down:
relation:
  - "[[应用层：HTTP]]"
  - "[[HTTP协议]]"
  - "[[context]]"
---

# Go net-http包

`net/http` 是 Go 标准库提供的 HTTP 客户端和服务端包。

## 服务端

```go
http.HandleFunc("/ping", func(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("pong"))
})
http.ListenAndServe(":8080", nil)
```

## 客户端

1. `http.Get` 适合简单请求。
2. 生产环境通常复用 `http.Client`。
3. 请求超时和取消应结合 `context`。

## 关注点

1. `Request.Body` 需要及时关闭。
2. 默认 Transport 会复用连接。
3. 服务端 handler 每个请求通常在独立 goroutine 中处理。
