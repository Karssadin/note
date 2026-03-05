---
tags:
  - C++
up:
  - "[[类型转换]]"
down:
relation:
---

- `static_cast < type-id > (expression)`
- 没有运行时检查类型检查来保证转换的安全性
- 进行上行转换（把派生类的指针或引用转换成基类表示）是安全的
- 进行下行转换（把基类的指针或引用转换为派生类表示），由于没有动态类型检查，所以是不安全的。
    - 用于基本数据类型之间的转换，如把int转换成char。
    - 把任何类型的表达式转换成void类型
- static_cast不能转换掉expression的const、volatile、或者__unaligned属性。

```C
double x = 3.14;
int y = static_cast<int>(x); // 安全的类型转换
```
