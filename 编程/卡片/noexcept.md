---
tags:
  - C++
up:
  - "[[C++新特性]]"
  - "[[异常处理]]"
down:
relation:
  - "[[构造函数和析构函数中能否抛出异常]]"
  - "[[移动语义]]"
  - "[[右值引用]]"
  - "[[异常处理]]"
---

C++11 引入 `noexcept` 关键字，替代 C++98 的 `throw()` 异常规格说明，用于声明函数不会抛出异常。

## 基本用法

```cpp
void safe_func() noexcept {
    // 保证不抛出异常
    // 如果内部抛异常，程序直接 std::terminate()
}

// 条件 noexcept
template <typename T>
void swap(T& a, T& b) noexcept(noexcept(T(std::move(a)))) {
    // 当 T 的移动构造不抛异常时，swap 也不抛
}
```

## noexcept 运算符

`noexcept(expr)` 是编译期运算符，返回 `true` / `false`：

```cpp
noexcept(42);                           // true
noexcept(std::string("hi"));            // false（可能分配内存）
noexcept(std::declval<int>() + 1);      // true
```

## 为什么重要

1. **移动语义优化**：STL 容器在扩容时，如果元素的移动构造是 `noexcept`，则使用移动而非拷贝

```cpp
class Widget {
public:
    Widget(Widget&&) noexcept = default;  // 标记 noexcept，vector 扩容时用移动
};
```

2. **编译器优化**：`noexcept` 函数不需要生成异常处理表，减小二进制体积
3. **接口契约**：明确告知调用者此函数不会抛异常

## 使用建议

- 移动构造/赋值、析构函数、swap 应标记 `noexcept`
- 不确定时不要随意加，违反 noexcept 会直接 terminate
- 析构函数默认隐式 `noexcept`
