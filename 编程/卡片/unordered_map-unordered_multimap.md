---
tags:
  - C++
  - STL
up:
  - "[[容器、适配器、工具]]"
down:
relation:
  - "[[map-multimap]]"
---

基于**哈希表**实现的键值对容器，元素无序，查找/插入/删除平均 O(1)。

## 底层原理

- 与 unordered_set 相同的拉链法哈希表，但每个节点存储 `pair<const Key, Value>`
- 负载因子超过阈值（默认 1.0）时自动 rehash，所有迭代器失效
- 桶内冲突多时退化为 O(n)

## 自定义类型作 key

需要提供 hash 函数和 `==` 运算符：

```cpp
struct Point {
    int x, y;
    bool operator==(const Point& o) const { return x == o.x && y == o.y; }
};

struct PointHash {
    size_t operator()(const Point& p) const {
        return std::hash<int>()(p.x) ^ (std::hash<int>()(p.y) << 16);
    }
};

std::unordered_map<Point, int, PointHash> mp;
```

## 与 map 的选择

- 需要有序遍历或范围查询 → `map`
- 只需快速查找、不关心顺序 → `unordered_map`
- 详见 [[map与unordered_map的区别]]

## 常用操作

```cpp
std::unordered_map<std::string, int> um;
um["key"] = 1;           // 插入或修改
um.insert({"k2", 2});
um.erase("key");
um.count("k2");           // 0 或 1
um.find("k2");            // 返回迭代器
um.bucket_count();
um.load_factor();
```

- `unordered_multimap` 允许重复 key，不支持 `[]` 操作符
