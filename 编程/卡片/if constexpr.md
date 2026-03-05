---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[constexpr函数]]"
  - "[[模板实例化与分支裁剪]]"
---

C++17 引入 `if constexpr`，在编译期根据条件选择分支，未选中的分支不参与编译。

## 解决的问题

C++11/14 中，模板函数内的 if 分支即使不会执行也必须通过编译：

```cpp
// C++11/14：两个分支都必须对 T 合法
template <typename T>
void process(T val) {
    if (std::is_integral<T>::value)
        val += 1;        // 对 string 不合法
    else
        val.append("x"); // 对 int 不合法
}
```

## C++17 解决方案

```cpp
template <typename T>
void process(T val) {
    if constexpr (std::is_integral_v<T>) {
        val += 1;         // 仅 T 为整数时编译
    } else {
        val.append("x");  // 仅 T 为非整数时编译
    }
}
```

## 替代 SFINAE

```cpp
// C++11 SFINAE 方式（复杂）
template <typename T>
std::enable_if_t<std::is_integral_v<T>, T> twice(T x) { return x * 2; }
template <typename T>
std::enable_if_t<!std::is_integral_v<T>, T> twice(T x) { return x + x; }

// C++17 if constexpr（简洁）
template <typename T>
T twice(T x) {
    if constexpr (std::is_integral_v<T>) return x * 2;
    else return x + x;
}
```

## 编译期递归终止

```cpp
template <typename T, typename... Args>
void print(T first, Args... rest) {
    std::cout << first;
    if constexpr (sizeof...(rest) > 0) {
        std::cout << ", ";
        print(rest...);
    }
}
```
