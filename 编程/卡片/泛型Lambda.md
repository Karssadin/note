---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[lambda]]"
---

C++14 允许 Lambda 参数使用 `auto`，使 Lambda 成为泛型的（等效于模板 `operator()`）。

## C++11 vs C++14

```cpp
// C++11：必须指定具体类型
auto add11 = [](int a, int b) { return a + b; };

// C++14：参数用 auto，自动推导
auto add14 = [](auto a, auto b) { return a + b; };

add14(1, 2);       // int
add14(1.5, 2.5);   // double
add14(std::string("a"), std::string("b")); // string
```

## 等价的模板形式

```cpp
// 泛型 Lambda 在编译器内部等效于：
struct Lambda {
    template <typename T, typename U>
    auto operator()(T a, U b) const { return a + b; }
};
```

## 常见用法

```cpp
// 泛型排序比较器
std::sort(v.begin(), v.end(), [](const auto& a, const auto& b) {
    return a < b;
});

// 配合 STL 算法
std::for_each(v.begin(), v.end(), [](const auto& x) {
    std::cout << x << "\n";
});
```

## C++20 进一步增强

C++20 引入模板 Lambda：`[]<typename T>(T x) { ... }`，可以显式约束类型。
