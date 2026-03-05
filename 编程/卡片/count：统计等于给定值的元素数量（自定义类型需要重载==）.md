---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
  - "[[count_if：统计满足条件的元素数量]]"
---
- 统计元素出现次数
- `count(iterator beg, iterator end， value);`
    - `beg` ：开始迭代器
    - `end`：结束迭代器
    - `value`：统计的元素值

```C
int n=count(v.begin(),v.end(),2)
```
