---
tags:
  - STL
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[类模板]]"
---

C++17 引入类模板实参推导（Class Template Argument Deduction, CTAD），允许从构造函数参数自动推导模板参数。

## 基本用法

```cpp
// C++14：必须显式指定模板参数
std::pair<int, double> p1(1, 2.0);
std::vector<int> v1 = {1, 2, 3};

// C++17：自动推导
std::pair p2(1, 2.0);        // pair<int, double>
std::vector v2 = {1, 2, 3};  // vector<int>
std::tuple t(1, 2.0, "hi");  // tuple<int, double, const char*>
std::mutex mtx;
std::lock_guard lg(mtx);     // lock_guard<std::mutex>
```

## 自定义推导指引

```cpp
template <typename T>
struct Container {
    Container(T) {}
    Container(std::initializer_list<T>) {}
};

// 推导指引
Container(const char*) -> Container<std::string>;

Container c1(42);       // Container<int>
Container c2("hello");  // Container<std::string>（走推导指引）
```

## 限制

- 不能部分推导：`std::pair<int> p(1, 2.0);` 不合法
- 聚合类型需要 C++20 才支持 CTAD
- `std::array` 在 C++17 中不支持 CTAD（C++20 支持）
