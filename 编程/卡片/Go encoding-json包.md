---
tags:
  - go
  - 标准库
up:
  - "[[Go标准库]]"
down:
relation:
  - "[[Go结构体]]"
  - "[[Go接口]]"
---

# Go encoding-json包

`encoding/json` 用于 JSON 编解码，常与结构体 tag 配合使用。

```go
type User struct {
	ID   int    `json:"id"`
	Name string `json:"name"`
}
```

## 常用函数

1. `json.Marshal`
2. `json.Unmarshal`
3. `json.NewEncoder`
4. `json.NewDecoder`

## 注意

1. 只有导出字段才会参与默认编解码。
2. `omitempty` 会在字段为空值时省略。
3. 数字解码到 `interface{}` 时默认使用 `float64`。
4. 流式场景优先使用 Encoder / Decoder。
