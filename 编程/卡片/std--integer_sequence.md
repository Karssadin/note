---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[可变参数模板]]"
---

C++14 引入 `std::integer_sequence`，用于在编译期生成整数序列，常配合 `std::tuple` 使用。

## 基本定义

```cpp
template <typename T, T... Ints>
struct integer_sequence {};

// 便捷别名
template <size_t... Ints>
using index_sequence = integer_sequence<size_t, Ints...>;

// 生成 0, 1, ..., N-1 的序列
template <size_t N>
using make_index_sequence = /* 编译器实现 */;
```

## 典型用途：展开 tuple

```cpp
template <typename Tuple, size_t... Is>
void print_impl(const Tuple& t, std::index_sequence<Is...>) {
    ((std::cout << std::get<Is>(t) << " "), ...);
}

template <typename... Args>
void print_tuple(const std::tuple<Args...>& t) {
    print_impl(t, std::make_index_sequence<sizeof...(Args)>{});
}
```

## 应用场景

- 展开 tuple 元素
- 编译期循环展开
- 参数包按索引访问
- 生成编译期查找表
