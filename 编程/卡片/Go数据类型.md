---
tags:
  - go
up:
  - "[[Go]]"
down:
  - "[[Go数组和切片]]"
  - "[[Go map]]"
  - "[[Go结构体]]"
  - "[[Go make与new]]"
  - "[[Go string与rune]]"
relation:
  - "[[数据类型]]"
  - "[[结构体：struct]]"
  - "[[指针]]"
---

# Go数据类型

Go 的数据类型分为基础类型、复合类型、引用语义类型和接口类型。理解值语义与引用语义是判断拷贝成本、并发安全和逃逸行为的基础。

1. 基础类型
	1. `bool`
	2. `int`、`uint`、`uintptr`
	3. `float32`、`float64`
	4. `complex64`、`complex128`
	5. [[Go string与rune]]
2. 复合类型
	1. [[Go数组和切片]]
	2. [[Go map]]
	3. [[Go结构体]]
3. 其他类型
	1. 指针：有指针但不支持指针运算
	2. 函数：函数是一等值
	3. 接口：通过方法集合描述行为
4. [[Go make与new]]
