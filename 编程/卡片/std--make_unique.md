---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[智能指针]]"
---

C++14 补齐了 C++11 缺失的 `std::make_unique`，提供与 `std::make_shared` 对称的工厂函数。

## 为什么需要

C++11 中创建 `unique_ptr` 只能用 `new`：

```cpp
// C++11：裸 new
auto p = std::unique_ptr<Widget>(new Widget(1, 2));

// 异常安全问题：
foo(std::unique_ptr<A>(new A), std::unique_ptr<B>(new B));
// 如果第一个 new 成功、第二个 new 抛异常，第一个可能泄漏
```

## C++14 用法

```cpp
// C++14：安全、简洁
auto p = std::make_unique<Widget>(1, 2);

// 数组版本
auto arr = std::make_unique<int[]>(10);  // 10 个 int

// 异常安全
foo(std::make_unique<A>(), std::make_unique<B>());
```

## make_unique vs make_shared

| 特性 | `make_unique` | `make_shared` |
|------|---------------|---------------|
| 返回类型 | `unique_ptr<T>` | `shared_ptr<T>` |
| 内存分配 | 一次 | 一次（控制块 + 对象合并） |
| 自定义删除器 | 不支持 | 不支持 |
| 弱引用延长内存 | 不适用 | 可能延长（控制块合并时） |
