---
tags:
  - 编程
up:
  - "[[C++]]"
down:
  - "[[STL概述]]"
  - "[[容器、适配器、工具]]"
  - "[[函数对象-仿函数]]"
  - "[[STL常用算法]]"
  - "[[vector与list的区别]]"
  - "[[map与unordered_map的区别]]"
relation:
  - "[[数据结构与算法]]"
---

# [[STL概述]]

1. [[STL优点]]
2. [[迭代器失效]]

# 序列容器

1. [[vector]]
2. [[list]]
3. [[deque]]
4. [[string]]
5. [[vector与list的区别]]

# 关联容器

1. [[set-multiset]]
	1. [[unordered_set-unordered_multiset]]
2. [[map-multimap]]
	1. [[unordered_map-unordered_multimap]]
3. [[map与unordered_map的区别]]
4. [[红黑树]]

# 容器适配器

1. [[stack]]
2. [[queue]]
3. [[priority_queue]]

# 工具

1. [[pair]]
2. [[bitset]]

# [[函数对象-仿函数]]

1. [[容器存储内置数据类型指定排序规则（利用仿函数）]]
2. [[容器存储自定义数据类型指定排序规则]]
3. [[lambda：使用lambda代替cmp]]
4. [[函数对象]]
5. [[内建函数对象]]
6. [[谓词]]

# [[STL常用算法]]

详见 [[STL常用算法]]，按类别分为 14 组（遍历/查找/统计/排序/二分/分区/随机反转/拷贝修改/填充/数值/最值/集合/堆/排列）。

常用算法速查：
1. [[for_each：对序列中每个元素执行指定操作]]、[[transform：对序列中每个元素执行指定操作并将结果存储到另一个序列中]]
2. [[find：查找第一个等于给定值的元素（自定义类型需要重载==）]]、[[find_if：查找第一个满足条件的元素]]
3. [[count：统计等于给定值的元素数量（自定义类型需要重载==）]]
4. [[sort：对序列进行排序]]、[[binary_search：检查序列中是否包含给定值]]
5. [[reverse：反转序列指定范围内的元素]]、[[copy：将序列指定范围内的元素复制到另一序列（注意resize）]]
6. [[replace：将序列指定范围内的旧元素修改为新元素]]、[[swap：交换两个容器的元素]]
7. [[fill：将指定值填充到序列指定范围内]]、[[accumulate：计算序列指定范围内的元素累积总和]]
8. [[merge：合并两个有序序列]]、[[set_union：计算两个序列的并集]]、[[set_intersection：计算两个序列的交集]]、[[set_difference：计算两个序列的差集]]
