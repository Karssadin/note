---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[模板实例化与分支裁剪]]"
---

type_traits 是 C++ 标准库提供的编译期类型检查与变换工具集，位于 `<type_traits>` 头文件中。通过 type_traits 可以在编译期查询类型属性（如 `std::is_integral`、`std::is_pointer`）或进行类型变换（如 `std::remove_const`、`std::decay`），常与 SFINAE、`if constexpr` 配合实现模板元编程。
