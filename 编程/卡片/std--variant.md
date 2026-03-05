---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
---

C++17 引入 `std::variant<Types...>`，类型安全的 union，编译期可检查所有可能类型。

## 替代 C 风格 union

```cpp
// C 风格 union：不安全
union U { int i; double d; std::string s; };  // UB：string 有析构函数

// C++17 variant：安全
std::variant<int, double, std::string> v;
v = 42;
v = 3.14;
v = "hello";  // 自动调用析构和构造
```

## 访问值

```cpp
std::variant<int, double, std::string> v = "hello";

// 方式一：std::get
std::string& s = std::get<std::string>(v);
// std::get<int>(v);  // 抛 std::bad_variant_access

// 方式二：std::get_if
if (auto* p = std::get_if<int>(&v)) {
    std::cout << *p;
}

// 方式三：std::visit（推荐）
std::visit([](auto&& arg) {
    std::cout << arg << "\n";
}, v);
```

## 典型场景

- 解析器中的 Token 类型：`variant<int, double, string, Symbol>`
- 状态机的状态表示
- 替代继承层次中简单的多态分发
