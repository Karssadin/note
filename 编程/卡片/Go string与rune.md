---
tags:
  - go
up:
  - "[[Go数据类型]]"
down:
relation:
  - "[[字符串常用操作]]"
  - "[[Go encoding-json包]]"
---

# Go string与rune

Go 的 `string` 是只读字节序列，通常保存 UTF-8 编码文本。`rune` 是 `int32` 的别名，表示一个 Unicode 码点。

## string

1. `len(s)` 返回字节数，不是字符数。
2. 字符串不可变，修改时通常转换为 `[]byte` 或 `[]rune`。
3. 字符串切片按字节截取，可能截断 UTF-8 字符。

## byte 和 rune

1. `byte` 是 `uint8` 的别名，适合处理原始字节。
2. `rune` 是 `int32` 的别名，适合处理 Unicode 码点。
3. `for range` 遍历字符串时，得到的是字节下标和 rune。

```go
for i, r := range "你好" {
	fmt.Println(i, r)
}
```

## 常见坑

1. 中文字符的字节数通常大于 1。
2. 需要按用户感知字符处理时，rune 也不一定等于完整字素簇。
3. 大量字符串拼接优先使用 `strings.Builder`。
