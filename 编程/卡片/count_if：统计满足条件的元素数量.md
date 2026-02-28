---
tags: 
up: 
  - "[[STL常用算法]]"
down: 
relation:
  - "[[count：统计等于给定值的元素数量（自定义类型需要重载==）]]"
---
- 按条件统计元素出现次数
- `count_if(iterator beg, iterator end，_Pred);`
    
    - `beg`：开始迭代器
    - `end`：结束迭代器
    - `_Pred`：谓词