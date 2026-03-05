---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
  - "[[set_difference：计算两个序列的差集]]"
  - "[[set_intersection：计算两个序列的交集]]"
  - "[[merge：合并两个有序序列]]"
---
- 注意:两个集合必须是有序序列
- 目标容器需要提前开辟空间，取序列大小相和即可
- 返回最后的值位置，可能不是`v.end()`
- `set_union(iterator beg1，iterator end1，iterator beg2，iterator end2,iterator dest);`
    - `beg1`：容器1开始迭代器
    - `end1`：容器1结束迭代器
    - `beg2`：容器2开始迭代器
    - `end2`：容器2结束迭代器
    - `dest`：目标容器开始迭代器
