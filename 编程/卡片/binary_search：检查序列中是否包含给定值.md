---
tags: 
up: 
  - "[[STL常用算法]]"
down: 
relation:
  - "[[find：查找第一个等于给定值的元素（自定义类型需要重载==）]]"
---
- 查找指定的元素，查到返回`true`否则`false`
- 在无序序列中不可用
- `bool bihary_search(iterator beg, iterator end,value) ;`
    - `beg`：开始迭代器
    - `end`：结束迭代器
    - `value`：查找的元素

```C
bool ret=binary_search(v.begin(),v.end(),9);
```