---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
  - "[[unordered_set-unordered_multiset]]"
---
- [[#常用迭代器|常用迭代器]]
- [[#构造和赋值|构造和赋值]]
- [[#大小和交换size();|大小和交换size();]]
- [[#插入和删除（只有insert和erase）|插入和删除（只有insert和erase）]]
- [[#查找和统计|查找和统计]]
- [[#set和multiset的区别|set和multiset的区别]]

----
- 基于平衡二叉树（红黑树）
- 动态维护有序序列
- **在插入时自动排序**，
- 属于关联式容器，底层结构使用二叉树实现
- `set`**不允许容器中有重复的元素**，`multiset`允许容器中有重复的元素

### 常用迭代器

- `begin()、end()`
- `find(key);`查找key是否存在,若存在，返回该键的元素的迭代器;若不存在，返回set.end();
- `lower_bound(x)`返回>=x的最小数的迭代器
- `upper_bount(x)`返回>x的最小数的迭代器

### 构造和赋值

- 构造:
    - `set<T> st;` 默认构造函数:
    - `set(const set &st);`拷贝构造函数
- 赋值:
    - `set& operator=(const set &st);`重载等号操作符

### 大小和交换size();

- 没有resize操作，因为指定大了的话，用0填充会有重复
- `size()` 返回容器中元素的数目
- `empty();` 判断容器是否为空
- `count()` 返回某个数的个数
- `swap(st);` 交换两个集合容器

### 插入和删除（只有insert和erase）

- `insert(elem);` 在容器中插入元素。
- `clear();` 清除所有元素
- `erase(pos);` 删除pos迭代器所指的元素，返回下一个元素的迭代器。
- `erase(beg, end);`删除区间[beg,end)的所有元素，返回下一个元素的迭代器。
- `erase(elem);` 删除容器中值为elem的元素。

### 查找和统计

- `find(key);`查找key是否存在,若存在，返回该键的元素的迭代器;若不存在，返回set.end();
- `count(key)` 统计key的元素个数

### set和multiset的区别

- set不可以插入重复数据，而multiset可以
- set插入数据的同时会返回插入结果（Pair类型，<迭代器，bool>，用.first或者.second来选择），表示插入是否成功
- multiset不会检测数据，因此可以插入重复数据，返回只返回迭代器