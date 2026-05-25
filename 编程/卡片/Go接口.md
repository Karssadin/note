---
tags:
  - go
up:
  - "[[Go方法与接口]]"
down:
relation:
  - "[[纯虚函数、抽象类、接口类]]"
  - "[[面向对象]]"
  - "[[Go接口与C++抽象类对比]]"
---

# Go接口

Go 接口通过方法集合描述行为，类型只要实现了接口要求的方法，就隐式满足该接口。

```go
type Reader interface {
	Read(p []byte) (n int, err error)
}
```

## 特点

1. 隐式实现，不需要 `implements` 关键字。
2. 小接口更常见，例如 `io.Reader`、`io.Writer`。
3. 空接口 `interface{}` 可以接收任意类型，Go 1.18 后常写作 `any`。
4. 接口值包含动态类型和动态值。
5. 接口值为 nil 要同时满足动态类型和值都为 nil。

## 常见操作

1. 类型断言：`v, ok := x.(T)`
2. type switch：按动态类型分支处理
3. 组合接口：接口可以嵌入其他接口
