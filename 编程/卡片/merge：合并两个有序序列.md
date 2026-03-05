---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
  - "[[set_difference：计算两个序列的差集]]"
  - "[[set_union：计算两个序列的并集]]"
  - "[[set_intersection：计算两个序列的交集]]"
---
  

- 容器元素合并，并存储到另一容器中
- 注意:两个容器必须是有序的
- `merge(iterator beg1，iterator end1， iterator beg2，iterator end2, iterator dest);`
    - `beg1`：容器1开始迭代器
    - `end1`：容器1结束迭代器
    - `beg2`：容器2开始迭代器
    - `end2`：容器2结束迭代器
    - `dest`：目标容器开始迭代器
