---
tags:
  - 操作系统
  - Linux
up:
  - "[[数据库基础知识]]"
down:
relation:
  - "[[SQL基础语法]]"
  - "[[窗口函数]]"
  - "[[SQL高级特性]]"
---

SQL 中 SELECT 语句的多种复用与组合查询方式。

## 子查询（Subquery）

将一个查询的结果作为另一个查询的输入。

```sql
SELECT name FROM employees
WHERE dept_id IN (SELECT id FROM departments WHERE location = 'Beijing');
```

- **标量子查询**：返回单个值
- **列子查询**：返回一列，配合 IN / ANY / ALL
- **行子查询**：返回一行
- **表子查询**：返回多行多列，用在 FROM 子句中

## 联合查询（UNION）

将多个 SELECT 结果纵向合并。

```sql
SELECT name FROM staff_a
UNION ALL          -- UNION 去重，UNION ALL 保留重复
SELECT name FROM staff_b;
```

## 多表连接（JOIN）

```sql
SELECT a.name, b.dept_name
FROM employees a
INNER JOIN departments b ON a.dept_id = b.id;
```

| 类型 | 说明 |
|------|------|
| INNER JOIN | 仅返回两表匹配行 |
| LEFT JOIN | 返回左表全部 + 右表匹配行 |
| RIGHT JOIN | 返回右表全部 + 左表匹配行 |
| CROSS JOIN | 笛卡尔积 |

## EXISTS vs IN

- `EXISTS`：检查子查询是否有结果，适合子查询结果集大的场景
- `IN`：匹配值列表，适合子查询结果集小的场景
