---
tags:
  - go
up:
  - "[[Go数据类型]]"
down:
relation:
  - "[[Go数组和切片]]"
  - "[[Go map]]"
  - "[[channel]]"
---

# Go make与new

`new` 和 `make` 都能用于创建值，但用途不同。

## new

```go
p := new(int)
```

1. 分配一块零值内存。
2. 返回指向该类型的指针 `*T`。
3. 适用于任意类型。

## make

```go
s := make([]int, 0, 8)
m := make(map[string]int)
ch := make(chan int, 1)
```

1. 只用于 slice、map、channel。
2. 返回初始化后的类型本身，不是指针。
3. 会初始化运行时内部结构，使其可直接使用。

## 对比

1. `new([]int)` 返回 `*[]int`，切片本身仍是 nil。
2. `make([]int, 0)` 返回可用的空切片。
3. nil map 不能写入，使用前需要 `make`。
