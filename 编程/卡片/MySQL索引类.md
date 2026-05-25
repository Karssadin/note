---
tags:
  - MySQL
up:
  - "[[索引]]"
down:
relation:
  - "[[索引概念]]"
  - "[[索引创建时机？创建-避免创建]]"
  - "[[InnoDB存储引擎]]"
---

MySQL 中索引的分类方式和底层实现：

## 按数据结构分

| 类型 | 实现 | 特点 |
|------|------|------|
| B+ 树索引 | InnoDB 默认 | 范围查询高效，有序 |
| 哈希索引 | Memory 引擎 / 自适应哈希 | 等值查询 O(1)，不支持范围 |
| 全文索引 | InnoDB（5.6+）/ MyISAM | 文本搜索，倒排索引实现 |
| R-Tree | MyISAM | 空间数据索引 |

## 按逻辑功能分

1. **主键索引**（PRIMARY KEY）：唯一且非空，InnoDB 中即聚簇索引
2. **唯一索引**（UNIQUE）：列值唯一，允许 NULL
3. **普通索引**（INDEX）：无唯一性约束
4. **组合索引**：多列联合索引，遵循最左前缀原则
5. **前缀索引**：对字符串列的前 N 个字符建索引，节省空间

## 按物理存储分

- **聚簇索引**（Clustered Index）：数据行与索引存储在一起，InnoDB 主键索引
- **非聚簇索引**（Secondary Index）：索引叶节点存储主键值，需要回表查询

## 覆盖索引

查询的所有列都包含在索引中，无需回表，性能最优。

```sql
-- 组合索引 (name, age)
SELECT name, age FROM users WHERE name = 'Alice';  -- 覆盖索引，不回表
SELECT *     FROM users WHERE name = 'Alice';       -- 需要回表
```
