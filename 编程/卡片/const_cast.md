---
tags:
up:
  - "[[类型转换]]"
down:
relation:
---

- 常量指针转换为非常量指针，并且仍然指向原来的对象。常量引用被转换为非常量引用，并且仍然指向原来的对象。
- 可以在const对象调用const函数时，如果要对const对象中的一些变量进行修改时，可以使用const_cast来进行转换，可以对其进行操作，但这样属于不安全行为，尽量不使用，可以对变量添加mutable。
- 去掉类型的const或volatile属性
- `const_cast (expression)`

```C
const int x = 10;
int* mutablePtr = const_cast<int*>(&x);  // 去除 const 属性
*mutablePtr = 20; // 但这样操作是不安全的！
```