---
tags:
  - C++
up:
  - "[[函数对象-仿函数]]"
down:
relation:
  - "[[lambda]]"
---

Lambda 表达式可以替代传统的仿函数类作为比较器，代码更简洁直观。

## lambda 语法回顾

```
[capture list](parameter list) -> return type { function body }
```

- **捕获列表**：`[&]` 全部引用捕获、`[=]` 全部值捕获、`[&x, y]` 混合捕获
- **参数列表**和**函数体**与普通函数相同
- **返回类型**通常可省略，编译器自动推导

## 在 sort 中使用

```cpp
// 仿函数写法
struct Cmp {
    bool operator()(int a, int b) { return a > b; }
};
sort(v.begin(), v.end(), Cmp());

// lambda 写法（等价）
sort(v.begin(), v.end(), [](int a, int b) { return a > b; });
```

## 在 priority_queue 中使用

```cpp
// priority_queue 需要传类型，lambda 需要 decltype
auto cmp = [](int a, int b) { return a > b; };
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);  // 小根堆
```

## 捕获外部变量

```cpp
vector<int> order = {3, 1, 2};
// 按 order 中的值排序另一个数组
sort(indices.begin(), indices.end(), [&order](int a, int b) {
    return order[a] < order[b];
});
```

仿函数无法直接访问外部变量，需要通过构造函数传入；lambda 的捕获列表解决了这个问题。

## 注意事项

- `sort` 的比较器必须满足**严格弱序**：`cmp(a,a)` 必须返回 false
- C++20 起可以直接用 lambda 作为模板参数（无需 decltype）
