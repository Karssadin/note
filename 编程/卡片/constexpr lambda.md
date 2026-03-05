---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[lambda]]"
  - "[[constexpr函数]]"
---

C++17 允许 Lambda 表达式用于编译期求值（`constexpr` 上下文）。

## 基本特性

C++17 中，如果 Lambda 满足 `constexpr` 函数的要求，它自动成为 `constexpr`：

```cpp
auto square = [](int x) { return x * x; };

constexpr int result = square(5);  // 编译期计算：25
static_assert(square(3) == 9);     // OK
```

## 显式标记

```cpp
auto factorial = [](int n) constexpr {
    int result = 1;
    for (int i = 2; i <= n; i++)
        result *= i;
    return result;
};

constexpr int f5 = factorial(5);  // 120，编译期
int runtime_val = factorial(n);   // 也可运行期使用
```

## 编译期数组初始化

```cpp
constexpr auto make_array = [](int start) {
    std::array<int, 5> arr{};
    for (int i = 0; i < 5; i++)
        arr[i] = start + i;
    return arr;
};

constexpr auto arr = make_array(10);  // {10, 11, 12, 13, 14}
```

## 与 C++20

C++20 进一步放宽限制，允许 constexpr Lambda 中使用 `new/delete` 和虚函数调用。
