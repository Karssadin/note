---
tags:
  - go
  - 标准库
up:
  - "[[Go标准库]]"
down:
relation:
  - "[[Go接口]]"
  - "[[文件操作]]"
---

# Go io包

`io` 包以小接口为核心，最重要的是 `Reader` 和 `Writer`。

```go
type Reader interface {
	Read(p []byte) (n int, err error)
}
```

## 常见接口

1. `io.Reader`
2. `io.Writer`
3. `io.Closer`
4. `io.Seeker`
5. `io.ReaderAt` / `io.WriterAt`

## 常用函数

1. `io.Copy`
2. `io.ReadAll`
3. `io.MultiReader`
4. `io.TeeReader`

## 设计特点

小接口让文件、网络连接、缓冲区、压缩流等对象可以统一组合。
