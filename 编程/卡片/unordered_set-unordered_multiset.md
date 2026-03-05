---
tags:
up:
  - "[[容器、适配器、工具]]"
down:
relation:
  - "[[set-multiset]]"
---

基于**哈希表**实现的集合容器，元素无序，查找/插入/删除平均 O(1)。

## 底层原理

- 哈希表由一组**桶（bucket）**组成，每个桶是一个链表（拉链法）
- 插入时：对元素计算哈希值 → 取模得到桶编号 → 插入到对应桶的链表中
- 查找时：同样计算桶编号 → 遍历桶内链表
- **负载因子**（load_factor）= 元素数 / 桶数，超过 `max_load_factor`（默认 1.0）时自动 rehash

## 与 set 的选择

| 维度 | set | unordered_set |
|------|-----|---------------|
| 底层 | 红黑树 | 哈希表 |
| 元素顺序 | 有序 | 无序 |
| 查找 | O(log n) | 平均 O(1)，最坏 O(n) |
| 范围查询 | 支持 lower_bound/upper_bound | 不支持 |
| 自定义类型 | 需要 `<` 运算符 | 需要 hash 函数和 `==` |

## 常用操作

```cpp
std::unordered_set<int> us = {1, 2, 3};
us.insert(4);
us.erase(2);
us.count(3);          // 1（存在）或 0
us.find(3);           // 返回迭代器
us.bucket_count();    // 桶数量
us.load_factor();     // 当前负载因子
us.reserve(100);      // 预分配桶
```

- `unordered_multiset` 允许重复元素，`count()` 可能返回 > 1
