---
tags:
  - C++
up:
  - "[[变量和常量]]"
down:
  - "[[constexpr函数]]"
  - "[[constexpr lambda]]"
relation:
  - "[[constexpr和const的区别]]"
  - "[[常量：define、const]]"
  - "[[const和define的区别]]"
---

`constexpr` 是 C++11 引入的关键字，用于声明编译期常量或编译期可求值的表达式。

## 与 const 的区别

- `const`：运行期常量，值在初始化后不可修改，但初始值可以是运行期计算的
- `constexpr`：编译期常量，值必须在编译期确定

```cpp
const int a = get_value();       // OK，运行期确定
constexpr int b = 42;            // OK，编译期确定
constexpr int c = get_value();   // 仅当 get_value() 也是 constexpr 时合法
```

## 使用场景

1. **数组大小**：`constexpr int N = 100; int arr[N];`
2. **模板参数**：`std::array<int, N>`
3. **替代宏常量**：类型安全，有作用域，可调试
4. **编译期计算**：配合 `constexpr` 函数在编译期完成复杂计算

## 版本演进

| 版本 | 能力 |
|------|------|
| C++11 | 仅支持单条 return 语句 |
| C++14 | 允许 if、for、局部变量 |
| C++17 | constexpr lambda、if constexpr |
| C++20 | constexpr new/delete、虚函数、try-catch |

## 与 `#define` 对比

| 特性 | `#define` | `constexpr` |
|------|-----------|-------------|
| 类型安全 | 无 | 有 |
| 作用域 | 全局 | 遵循 C++ 作用域 |
| 调试 | 不可见 | 可见 |
| 编译期求值 | 是（文本替换） | 是（真正的常量表达式） |
