---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[函数模板]]"
  - "[[type_traits]]"
  - "[[if constexpr]]"
---

SFINAE（Substitution Failure Is Not An Error）是 C++ 模板的核心机制：模板参数替换失败时不报错，而是从候选集中移除该模板。`std::enable_if` 是基于 SFINAE 的标准工具。

## SFINAE 原理

```cpp
template <typename T>
typename T::value_type get_value(T t) { return t[0]; }

template <typename T>
T get_value(T t) { return t; }

get_value(std::vector<int>{1, 2});  // 选第一个：vector 有 value_type
get_value(42);                       // 选第二个：int 没有 value_type，第一个 SFINAE 掉
```

## std::enable_if

```cpp
// 基本定义
template <bool Cond, typename T = void>
struct enable_if {};

template <typename T>
struct enable_if<true, T> { using type = T; };

// 用法：仅当 T 是整数类型时启用
template <typename T>
std::enable_if_t<std::is_integral_v<T>, T>
twice(T x) { return x * 2; }

template <typename T>
std::enable_if_t<std::is_floating_point_v<T>, T>
twice(T x) { return x + x; }
```

## C++17 替代：if constexpr

```cpp
// 更简洁的写法
template <typename T>
T twice(T x) {
    if constexpr (std::is_integral_v<T>) return x * 2;
    else return x + x;
}
```

C++20 进一步引入 `concept` / `requires`，让模板约束更直观。
