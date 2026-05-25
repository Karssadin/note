---
tags:
  - go
  - 并发
up:
  - "[[Go并发编程]]"
down:
relation:
  - "[[协程]]"
  - "[[用户级线程与内核级线程]]"
  - "[[Go GMP调度模型]]"
---

# goroutine

goroutine 是 Go 的轻量级并发执行单元，由 Go runtime 调度到 OS 线程上运行。

```go
go func() {
	doWork()
}()
```

## 特点

1. 创建成本低，初始栈较小并可动态增长。
2. 由 runtime 调度，不等同于 OS 线程。
3. 阻塞系统调用、网络 IO、channel 操作都可能影响调度。
4. 需要用 channel、context、WaitGroup 等手段管理生命周期。

## 常见问题

1. goroutine 泄露：启动后没有退出路径。
2. 闭包捕获循环变量导致结果异常。
3. 未处理 panic 会导致程序崩溃。
