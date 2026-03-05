---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[decltype——表达式类型推导]]"
---

C++14 引入 `decltype(auto)`，精确保留表达式的值类别（左值引用、右值等），常用于泛型转发场景。

## 与 auto 的区别

```cpp
int x = 42;
int& rx = x;

auto a = rx;            // int（auto 会去掉引用）
decltype(auto) b = rx;  // int&（保留引用）

auto c = (x);            // int
decltype(auto) d = (x);  // int&（带括号的表达式是左值）
```

## 典型用途：完美返回

```cpp
template <typename F, typename... Args>
decltype(auto) wrapper(F&& f, Args&&... args) {
    return std::forward<F>(f)(std::forward<Args>(args)...);
}
```

如果用 `auto`，返回值引用会被丢弃；`decltype(auto)` 保留原函数的精确返回类型。

## 注意

- `decltype(auto)` 变量必须立即初始化
- 不能用于函数参数
- 带括号的变量名 `(x)` 会被推导为引用
