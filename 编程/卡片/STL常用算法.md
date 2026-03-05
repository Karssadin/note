---
tags:
  - STL
up:
  - "[[STL]]"
down:
  - "[[for_each：对序列中每个元素执行指定操作]]"
  - "[[transform：对序列中每个元素执行指定操作并将结果存储到另一个序列中]]"
  - "[[find：查找第一个等于给定值的元素（自定义类型需要重载==）]]"
  - "[[find_if：查找第一个满足条件的元素]]"
  - "[[adjacent_find：查找相邻的重复元素]]"
  - "[[count：统计等于给定值的元素数量（自定义类型需要重载==）]]"
  - "[[count_if：统计满足条件的元素数量]]"
  - "[[sort：对序列进行排序]]"
  - "[[排序算法补充]]"
  - "[[binary_search：检查序列中是否包含给定值]]"
  - "[[二分查找算法]]"
  - "[[分区算法]]"
  - "[[random_shuffle：随机打乱序列的指定范围内元素]]"
  - "[[reverse：反转序列指定范围内的元素]]"
  - "[[copy：将序列指定范围内的元素复制到另一序列（注意resize）]]"
  - "[[swap：交换两个容器的元素]]"
  - "[[replace：将序列指定范围内的旧元素修改为新元素]]"
  - "[[replace_if：将序列指定范围内满足条件的元素修改为新元素]]"
  - "[[remove与erase惯用法]]"
  - "[[fill：将指定值填充到序列指定范围内]]"
  - "[[accumulate：计算序列指定范围内的元素累积总和]]"
  - "[[数值运算算法]]"
  - "[[最值算法]]"
  - "[[merge：合并两个有序序列]]"
  - "[[set_difference：计算两个序列的差集]]"
  - "[[set_intersection：计算两个序列的交集]]"
  - "[[set_union：计算两个序列的并集]]"
  - "[[堆操作算法]]"
  - "[[排列算法]]"
relation:
---

STL 算法分布在三个头文件中：
- `<algorithm>`：最大，包含比较、交换、查找、遍历、复制、修改等
- `<numeric>`：简单数学运算（accumulate、inner_product 等）
- `<functional>`：函数对象相关模板类

所有算法通过**迭代器**操作容器，不依赖容器类型。

## 1. 遍历

对序列中的每个元素执行操作。

1. [[for_each：对序列中每个元素执行指定操作]]
2. [[transform：对序列中每个元素执行指定操作并将结果存储到另一个序列中]]

## 2. 查找

在序列中搜索满足条件的元素，返回迭代器。

1. [[find：查找第一个等于给定值的元素（自定义类型需要重载==）]]
2. [[find_if：查找第一个满足条件的元素]]
3. [[adjacent_find：查找相邻的重复元素]]

## 3. 统计

1. [[count：统计等于给定值的元素数量（自定义类型需要重载==）]]
2. [[count_if：统计满足条件的元素数量]]

## 4. 排序

1. [[sort：对序列进行排序]]：不稳定排序，O(n log n)
2. [[排序算法补充]]：stable_sort、partial_sort、nth_element、is_sorted

## 5. 二分查找

要求序列**已排序**，O(log n)。

1. [[binary_search：检查序列中是否包含给定值]]
2. [[二分查找算法]]：lower_bound、upper_bound、equal_range

## 6. 分区

1. [[分区算法]]：partition、stable_partition、is_partitioned、partition_point

## 7. 随机与反转

1. [[random_shuffle：随机打乱序列的指定范围内元素]]（C++14 弃用，推荐 shuffle）
2. [[reverse：反转序列指定范围内的元素]]

## 8. 拷贝与修改

1. [[copy：将序列指定范围内的元素复制到另一序列（注意resize）]]
2. [[swap：交换两个容器的元素]]
3. [[replace：将序列指定范围内的旧元素修改为新元素]]
4. [[replace_if：将序列指定范围内满足条件的元素修改为新元素]]
5. [[remove与erase惯用法]]：remove、remove_if、unique + erase

## 9. 填充与生成

1. [[fill：将指定值填充到序列指定范围内]]

## 10. 数值运算

1. [[accumulate：计算序列指定范围内的元素累积总和]]
2. [[数值运算算法]]：iota、partial_sum、adjacent_difference、inner_product、reduce

## 11. 最值

1. [[最值算法]]：min/max、min_element/max_element、minmax、clamp

## 12. 集合操作

要求两个输入序列**已排序**。

1. [[merge：合并两个有序序列]]
2. [[set_union：计算两个序列的并集]]
3. [[set_intersection：计算两个序列的交集]]
4. [[set_difference：计算两个序列的差集]]

## 13. 堆操作

1. [[堆操作算法]]：make_heap、push_heap、pop_heap、sort_heap、is_heap

## 14. 排列

1. [[排列算法]]：next_permutation、prev_permutation
