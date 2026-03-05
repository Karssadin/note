---
tags:
  - C++
up:
  - "[[数据类型]]"
  - "[[常用函数、命名空间]]"
down:
relation:
  - "[[RTTI]]"
---

## typeid 运算符

`typeid` 是 C++ RTTI（运行时类型识别）的组成部分，头文件 `<typeinfo>`。

```cpp
#include <typeinfo>
#include <iostream>

int x = 42;
double d = 3.14;
std::string s = "hello";

std::cout << typeid(x).name() << std::endl;  // i (int)
std::cout << typeid(d).name() << std::endl;  // d (double)
std::cout << typeid(s).name() << std::endl;  // NSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEE
```

> `name()` 返回的字符串是实现定义的（mangled name），可用 `c++filt` 工具解码

## 使用场景

```cpp
// 判断对象实际类型（多态）
class Base { virtual void foo() {} };
class Derived : public Base {};

Base* ptr = new Derived();
if (typeid(*ptr) == typeid(Derived)) {
    std::cout << "ptr 实际指向 Derived" << std::endl;
}
```

## 与 dynamic_cast 的比较

| 工具 | 用途 | 失败返回 |
|------|------|---------|
| `typeid` | 获取/比较类型信息 | 不失败（但对 null 指针抛异常） |
| `dynamic_cast<T*>` | 安全向下转型 | 返回 `nullptr` |
| `dynamic_cast<T&>` | 安全向下转型（引用） | 抛 `std::bad_cast` |

## 注意事项

- 对多态类型（含虚函数）的**指针**，`typeid(*ptr)` 返回动态类型
- 对非多态类型，`typeid` 在**编译期**确定，不需要运行时开销
- 解引用空指针 `typeid(*null_ptr)` 会抛 `std::bad_typeid`
- 启用 RTTI 需要编译器不禁用（GCC 默认开启，`-fno-rtti` 会关闭）
