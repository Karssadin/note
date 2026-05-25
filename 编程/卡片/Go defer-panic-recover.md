---
tags:
  - go
up:
  - "[[Go基础知识]]"
  - "[[Go运行时]]"
down:
relation:
  - "[[DEFER]]"
  - "[[异常处理]]"
  - "[[RAII]]"
---

# Go defer-panic-recover

`defer` 用于注册延迟执行函数，常见于资源释放、解锁和统一收尾。`panic` 表示当前 goroutine 发生不可恢复错误，`recover` 可以在 defer 函数中捕获 panic。

```go
func readFile(name string) {
	f, err := os.Open(name)
	if err != nil {
		panic(err)
	}
	defer f.Close()
}
```

## defer

1. defer 参数会在注册时求值。
2. 多个 defer 按后进先出顺序执行。
3. return 不是原子过程，defer 会在返回值赋值后、函数真正返回前执行。

## panic 和 recover

1. panic 会沿调用栈展开并执行已注册的 defer。
2. recover 只有在 defer 函数中直接调用才有效。
3. 业务错误优先返回 `error`，不要滥用 panic。
