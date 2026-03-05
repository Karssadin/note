---
tags:
  - 算子
up:
  - "[[qtree_operator]]"
down:
  - "[[编程/卡片/qtree_NLJoin]]"
relation:
  - "[[join]]"
---
## 类型
- NLJoin、MergeJoin、HashJoin都是转换为HashJoin，因为只实现了HashJoin

- 等值Join为例子
- Inner Join：返回全部的匹配数据
- Left Outer Join：返回左表的全部数据，右表与左表匹配的数据输出，不匹配的填充NULL值输出
- Right Outer Join：返回右表的全部数据，右表与左表匹配的数据输出，不匹配的填充NULL值输出
- Full Outer Join：返回全部数据，匹配的放到一行，不匹配的填充NULL
- Left Semi Join：返回左表的所有匹配数据
- Left Anti Semi Join：返回左表的不匹配数据
- Left Anti Semi Join Not In：考虑 not in 中的null值，not in中的值有null 值时，会返回空
## 实现