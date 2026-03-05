---
tags:
up:
  - "[[容器、适配器、工具]]"
down:
relation:
---

`std::pair<T1, T2>` 是一个简单的二元组模板，用于将两个值绑定在一起。底层定义为 struct，成员 `first` 和 `second` 均为 public。

## 创建方式

```cpp
pair<int, string> p1(1, "hello");
pair<int, string> p2 = make_pair(2, "world");
pair<int, string> p3 = {3, "foo"};        // C++11 列表初始化

// 嵌套 pair
pair<int, pair<int, int>> nested(1, {2, 3});
```

## 访问与解包

```cpp
p1.first;   // 1
p1.second;  // "hello"

// C++17 结构化绑定
auto [id, name] = p1;
```

## 比较规则

pair 支持 `<`、`==` 等比较运算，先比较 `first`，`first` 相等再比较 `second`：

```cpp
make_pair(1, 2) < make_pair(1, 3);  // true（first 相等，比 second）
make_pair(1, 2) < make_pair(2, 0);  // true（first 不同，直接比 first）
```

## 在 STL 中的应用

- `map` 的元素类型就是 `pair<const Key, Value>`
- `map::insert` 返回 `pair<iterator, bool>`，second 表示插入是否成功
- `set::insert` 同理

## 与 tuple 的关系

- `pair` 是固定两个元素，`tuple` 支持任意多个元素
- `pair` 可以直接 `.first` / `.second`，`tuple` 需要 `std::get<N>()`
- `pair` 更轻量，两个元素时优先使用 `pair`
