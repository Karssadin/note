---
tags:
  - go
  - 数据结构
up:
  - "[[Go数据类型]]"
down:
relation:
  - "[[红黑树]]"
  - "[[哈希表]]"
  - "[[数据结构与算法]]"
---

# Go map

Go 的 `map` 是哈希表，用于保存 key 到 value 的映射。

```go
m := map[string]int{"a": 1}
v, ok := m["a"]
```

## 特点

1. key 必须是可比较类型，例如整数、字符串、指针、数组、结构体。
2. slice、map、function 不能作为 key。
3. 读取不存在的 key 会返回 value 类型零值。
4. 使用 `v, ok := m[key]` 区分零值和不存在。
5. map 不是并发安全的，并发读写需要加锁或使用 `sync.Map`。
6. 遍历顺序不保证稳定。
