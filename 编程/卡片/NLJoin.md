---
tags:
  - C++
up:
  - "[[join]]"
down:
  - "[[StarRocks_NLjoin]]"
  - "[[qtree_NLJoin]]"
relation:
  - "[[StarRocks_NLjoin]]"
  - "[[qtree_NLJoin]]"
---
## 简介
- NL Join适用于数据集较小，Hash Join的开销（构建哈希表内存消耗、时间成本）大于使用NL Join的成本时。
## 已知实现
1. [[StarRocks_NLjoin]]
2. [[qtree_NLJoin]]
