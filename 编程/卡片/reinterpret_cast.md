---
tags:
  - C++
up:
  - "[[类型转换]]"
down:
relation:
---

- 可以将整型转换为指针，也可以把指针转换为数组；可以在指针和引用里进行转换，平台移植性比价差
- `reinterpret_cast< type-id > (expression)`。type-id 必须是一个指针、引用、算术类型、函数指针或者成员指针。它可以用于类型之间进行强制转换。

```C
int x = 10;
long* ptr = reinterpret_cast<long*>(&x);  // int* 转为 long*
```
