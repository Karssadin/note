---
tags:
  - C++
up:
  - "[[完美转发]]"
down:
relation:
  - "[[右值引用]]"
  - "[[std--move]]"
  - "[[函数模板]]"
---

# std::forward

`std::forward<T>` 用于完美转发，根据模板参数 `T` 的推导结果保留实参原本的左值或右值属性。

```cpp
template <typename T>
void wrapper(T&& value) {
    process(std::forward<T>(value));
}
```

## 与 std::move 的区别

1. `std::move` 无条件转换为右值引用。
2. `std::forward<T>` 依赖 `T` 的推导结果，有条件地转发为左值或右值。
3. `std::forward` 主要用于转发引用和泛型代码。

## 使用条件

1. 通常出现在函数模板中。
2. 参数形式通常是 `T&&`。
3. 调用时必须显式写模板参数：`std::forward<T>(value)`。
