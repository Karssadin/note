---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
---

C++17 引入 `inline` 变量，允许在头文件中定义变量而不违反 ODR（One Definition Rule）。

## 解决的问题

C++14 中，头文件中定义全局变量会导致多重定义错误：

```cpp
// config.h —— C++14
extern const int MAX_SIZE;  // 声明
// config.cpp
const int MAX_SIZE = 100;   // 定义（必须在一个 .cpp 中）
```

## C++17 解决方案

```cpp
// config.h —— C++17
inline constexpr int MAX_SIZE = 100;  // 直接在头文件定义
```

多个翻译单元包含此头文件时，链接器保证只保留一份定义。

## 类静态成员

```cpp
// C++14：需要类外定义
struct Config {
    static const int value;
};
const int Config::value = 42;  // .cpp 中

// C++17：inline 直接在类内定义
struct Config {
    inline static constexpr int value = 42;
};
```

## 使用场景

- 头文件中的常量定义
- header-only 库中的全局配置
- 模板类/函数的静态变量
