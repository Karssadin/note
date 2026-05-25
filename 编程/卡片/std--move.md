---
tags:
  - C++
up:
  - "[[移动语义]]"
down:
relation:
  - "[[右值引用]]"
  - "[[std--forward]]"
---

# std::move

`std::move` 本身不移动任何资源，它只是把表达式无条件转换为右值引用，从而让重载决议选择移动构造或移动赋值。

```cpp
std::vector<int> a{1, 2, 3};
std::vector<int> b = std::move(a);
```

## 注意

1. 被 `std::move` 的对象仍然处于有效状态，但值通常未指定。
2. 基本类型使用 `std::move` 通常仍是复制。
3. 对 `const` 对象使用 `std::move` 通常无法触发移动，因为移动构造需要修改源对象。
4. 不要对即将被返回的局部变量盲目使用 `std::move`，可能影响 RVO/NRVO。
