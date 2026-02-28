---
tags: 
up:
  - "[[STL未整理]]"
down:
  - "[[for_each：对序列中每个元素执行指定操作]]"
  - "[[transform：对序列中每个元素执行指定操作并将结果存储到另一个序列中]]"
  - "[[find：查找第一个等于给定值的元素（自定义类型需要重载==）]]"
  - "[[find_if：查找第一个满足条件的元素]]"
  - "[[adjacent_find：查找相邻的重复元素]]"
  - "[[count：统计等于给定值的元素数量（自定义类型需要重载==）]]"
  - "[[count_if：统计满足条件的元素数量]]"
  - "[[sort：对序列进行排序]]"
  - "[[容器存储内置数据类型指定排序规则（利用仿函数）]]"
  - "[[容器存储自定义数据类型指定排序规则]]"
  - "[[binary_search：检查序列中是否包含给定值]]"
  - "[[random_shuffle：随机打乱序列的指定范围内元素]]"
  - "[[reverse：反转序列指定范围内的元素]]"
  - "[[copy：将序列指定范围内的元素复制到另一序列（注意resize）]]"
  - "[[swap：交换两个容器的元素]]"
  - "[[replace：将序列指定范围内的旧元素修改为新元素]]"
  - "[[replace_if：将序列指定范围内满足条件的元素修改为新元素]]"
  - "[[fill：将指定值填充到序列指定范围内]]"
  - "[[accumulate：计算序列指定范围内的元素累积总和]]"
  - "[[merge：合并两个有序序列]]"
  - "[[set_difference：计算两个序列的差集]]"
  - "[[set_intersection：计算两个序列的交集]]"
  - "[[set_union：计算两个序列的并集]]"
relation: 
---
- `<algorithm>`是所有STL头文件中最大的一个，范围涉及到比较、交换、查找、遍历操作、复制、修改等等
- `<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数
- `<functional>`定义了一些模板类,用以声明函数对象

1. 遍历
    1. [[for_each：对序列中每个元素执行指定操作]]
    2. [[transform：对序列中每个元素执行指定操作并将结果存储到另一个序列中]]
2. 查找
    1. [[find：查找第一个等于给定值的元素（自定义类型需要重载==）]]
    2. [[find_if：查找第一个满足条件的元素]]
    3. find_if_not
    4. find_end
    5. find_first_of
    6. [[adjacent_find：查找相邻的重复元素]]
    7. search
    8. search_n
3. 统计
    1. [[count：统计等于给定值的元素数量（自定义类型需要重载==）]]
    2. [[count_if：统计满足条件的元素数量]]
4. 排序
    1. [[sort：对序列进行排序]]
    2. [[容器存储内置数据类型指定排序规则（利用仿函数）]]
    3. [[容器存储自定义数据类型指定排序规则]]
    4. stable_sort
    5. partial_sort
    6. partial_sort_copy
    7. is_sorted
    8. is_sorted_until
    9. nth_element
5. 二分查找
    1. lower_bound
    2. upper_bound
    3. [[binary_search：检查序列中是否包含给定值]]
    4. equal_range
6. 分区
    1. is_parititioned
    2. partition
    3. partition_copy
    4. stable_partition
    5. partition_point
7. 随机
    1. [[random_shuffle：随机打乱序列的指定范围内元素]]
    2. shuffle
8. 反转
    1. [[reverse：反转序列指定范围内的元素]]
    2. reverse_copy
9. 拷贝
    1. [[copy：将序列指定范围内的元素复制到另一序列（注意resize）]]
    2. copy_if
    3. copy_n
    4. copy_backword
10. 修改
    1. [[swap：交换两个容器的元素]]
    2. [[replace：将序列指定范围内的旧元素修改为新元素]]
    3. [[replace_if：将序列指定范围内满足条件的元素修改为新元素]]
    4. replace_copy
    5. replace_copy_if
11. 填充
    1. [[fill：将指定值填充到序列指定范围内]]
    2. fill_n
    3. generate
    4. generate_n
    5. push_heap
12. 赋值
    1. iota
13. 求和
    1. [[accumulate：计算序列指定范围内的元素累积总和]]
    2. inner_product
    3. adjacent_difference
    4. partial_sum
14. 最值
    1. min
    2. max
    3. min_element
    4. max_element
    5. minmax
    6. minmax_element
    7. clamp (C++17起)
15. 集合操作
    1. inplace_merge
    2. includes
    3. [[merge：合并两个有序序列]]
    4. [[set_difference：计算两个序列的差集]]
    5. [[set_union：计算两个序列的并集]]
    6. [[set_intersection：计算两个序列的交集]]
    7. set_symmetric_difference
16. 堆操作
    1. push_heap
    2. pop_heap
    3. make_heap
    4. sort_heap
    5. is_heap
    6. is_heap_until
17. 数值操作
    1. reduce (C++17起)
    2. exclusive_scan (C++17起)
    3. inclusive_scan (C++17起)
    4. transform_reduce (C++17起)
    5. transform_exclusive_scan (C++17起)
    6. transform_inclusive_scan (C++17起)
18. 其他操作
    1. permutation (C++17前)
    2. next_permutation
    3. prev_permutation