---
tags:
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[循环语句：while、for]]"
---

C++11 引入范围 for 循环（Range-based for loop），简化容器和数组的遍历。

## 基本语法

```cpp
// C++98 传统写法
for (std::vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
    std::cout << *it << "\n";
}

// C++11 范围 for
for (int x : v) {
    std::cout << x << "\n";
}
```

## 引用与 const

```cpp
std::vector<std::string> words = {"hello", "world"};

for (auto x : words) {}          // 拷贝每个元素（开销大）
for (auto& x : words) {}         // 引用，可修改
for (const auto& x : words) {}   // const 引用（推荐只读场景）
```

## 适用对象

任何提供 `begin()` / `end()` 的对象，或可通过 ADL 找到 `begin` / `end` 自由函数：

```cpp
// 原生数组
int arr[] = {1, 2, 3};
for (int x : arr) {}

// 初始化列表
for (int x : {1, 2, 3}) {}

// 自定义类型
struct Range {
    int* begin() { return data; }
    int* end()   { return data + size; }
    int data[10]; int size;
};
```

## 等价展开

```cpp
for (auto& x : container) { body; }

// 等价于：
{
    auto&& __range = container;
    auto __begin = std::begin(__range);
    auto __end   = std::end(__range);
    for (; __begin != __end; ++__begin) {
        auto& x = *__begin;
        body;
    }
}
```

## C++20 增强

C++20 支持带初始化的范围 for：`for (auto v = get(); auto& x : v) {}`
