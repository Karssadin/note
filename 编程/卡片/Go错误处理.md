---
tags:
  - go
up:
  - "[[Go基础知识]]"
down:
relation:
  - "[[异常处理]]"
  - "[[Go defer-panic-recover]]"
---

# Go错误处理

Go 倾向于把错误作为普通返回值显式处理，而不是用异常作为主要控制流。

```go
v, err := do()
if err != nil {
	return err
}
```

## 常见方式

1. 返回 `error` 接口值。
2. 用 `errors.New` 或 `fmt.Errorf` 构造错误。
3. 用 `%w` 包装错误，配合 `errors.Is`、`errors.As` 判断错误链。
4. 对不可恢复错误才使用 `panic`。

## 优点

1. 控制流显式。
2. 调用者必须面对错误路径。
3. 与多返回值配合自然。
