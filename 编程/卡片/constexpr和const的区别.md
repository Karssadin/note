---
tags:
  - C++
up:
  - "[[constexpr常量]]"
down:
relation:
  - "[[static和const的区别]]"
  - "[[常量：define、const]]"
  - "[[const和define的区别]]"
---

> 本卡内容已整合至 [[constexpr常量]]，请移步查阅完整的 constexpr vs const 对比。

## 核心区别速查

| 维度 | `const` | `constexpr` |
|------|---------|-------------|
| 求值时机 | 运行期（可以是运行期值） | 编译期（必须编译期确定） |
| 用途 | 声明只读变量 | 声明编译期常量/函数 |
| 成员函数 | `const` 成员函数不修改对象 | `constexpr` 成员函数可用于常量表达式 |
| 指针 | `const int*` 或 `int* const` | `constexpr` 指针本身是编译期常量 |
| 函数 | 不可用于函数 | 可声明在编译期可求值的函数 |

- `const` 标识只读，`constexpr` 标识编译期常量
- `constexpr` 隐含 `const`，但 `const` 不隐含 `constexpr`

完整内容见 [[constexpr常量]]
