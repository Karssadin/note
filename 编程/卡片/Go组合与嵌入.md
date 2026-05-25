---
tags:
  - go
up:
  - "[[Go方法与接口]]"
down:
relation:
  - "[[Go结构体]]"
  - "[[继承]]"
  - "[[面向对象]]"
---

# Go组合与嵌入

Go 没有类继承，通常通过结构体嵌入和接口组合复用能力。

## 结构体嵌入

```go
type Logger struct{}

func (Logger) Info(msg string) {}

type Service struct {
	Logger
}
```

1. 匿名字段的方法会被提升到外层类型。
2. 方法提升不是继承，外层类型和内层类型仍是不同类型。
3. 字段或方法同名时，外层定义优先。
4. 嵌入适合复用实现，但不要用它模拟复杂继承层次。

## 接口组合

```go
type ReadWriter interface {
	io.Reader
	io.Writer
}
```

接口组合用于把小接口组合成更大的行为集合。
