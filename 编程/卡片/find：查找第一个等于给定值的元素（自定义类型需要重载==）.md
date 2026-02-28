---
tags: 
up: 
  - "[[STL常用算法]]"
down: 
relation:
  - "[[find_if：查找第一个满足条件的元素]]"
  - "[[adjacent_find：查找相邻的重复元素]]"
  - "[[binary_search：检查序列中是否包含给定值]]"
---
- 按值查找元素，找到**返回指定元素的迭代器**，找不到返回结束迭代器`end()`
- 如果查找自定义类型的话，需要重载`==`号
- `find(iterator beg, iterator end, value);`
    - beg开始迭代器
    - end结束迭代器
    - value查找的元素

```C
\#include<algorithm>

vector<int>::iterator it=find(v.begin(),v.end(),5);
if(it !=v.end())

vector<Person>::iterator it=find(v.begin(),v.end(),Person("ttt",12));
//需要给自定义数据类型重载==号
```