---
tags:
  - MySQL
up:
  - "[[编程/归档/八股文/MySQL]]"
down:
  - "[[索引]]"
  - "[[锁与并发控制]]"
  - "[[Buffer Pool]]"
relation:
  - "[[日志]]"
  - "[[事务]]"
---

## 慢查询定位

```sql
SET GLOBAL slow_query_log = ON;
SET GLOBAL long_query_time = 1;  -- 超过 1 秒记录
SHOW VARIABLES LIKE 'slow_query_log_file';  -- 查看日志路径
```

定位到慢 SQL 后，用 Explain 分析执行计划。

## Explain 分析

| 字段 | 关键值 | 说明 |
|------|--------|------|
| type | system > const > eq_ref > ref > range > index > ALL | 访问类型，ALL 最差（全表扫描） |
| key | 索引名 | 实际使用的索引，NULL 表示未用索引 |
| rows | 数字 | 预估扫描行数，越小越好 |
| Extra | Using index | 覆盖索引，无需回表 |
| Extra | Using filesort | 需额外排序，考虑加排序字段索引 |
| Extra | Using temporary | 使用临时表，考虑优化 GROUP BY / DISTINCT |
| Extra | Using where | 存储引擎返回后在 Server 层过滤 |

## SQL 优化要点

1. 避免 `SELECT *`，只查需要的列（减少网络传输、利于覆盖索引）
2. 利用覆盖索引减少回表
3. 联合索引遵循最左前缀原则，高区分度的列放前面
4. 避免在 WHERE 子句中对列使用函数（导致索引失效）
5. 小表驱动大表：`IN` 适合子查询结果集小，`EXISTS` 适合外表小
6. 避免隐式类型转换（varchar 列用 int 查询会导致索引失效）

## JOIN 优化

| 算法 | 说明 | 优化方向 |
|------|------|---------|
| Nested Loop Join（NLJ） | 驱动表每行去被驱动表查找 | 被驱动表必须有索引 |
| Block Nested Loop Join（BNL） | 驱动表数据加载到 join buffer 批量匹配 | 增大 `join_buffer_size` |

- 小表做驱动表（`STRAIGHT_JOIN` 可强制指定驱动顺序）
- 被驱动表的关联字段必须加索引
- 避免多表 JOIN（MySQL 优化器对 3 表以上 JOIN 的执行计划质量下降）

## 分页优化

大偏移量问题：`LIMIT 1000000, 10` 需要扫描 100 万行后丢弃前 100 万行。

### 延迟关联

```sql
SELECT * FROM t
INNER JOIN (SELECT id FROM t ORDER BY create_time LIMIT 1000000, 10) AS tmp
ON t.id = tmp.id;
```

子查询只扫覆盖索引，拿到主键后再回表，大幅减少 IO。

### 游标分页

```sql
-- 第一页
SELECT * FROM t WHERE id > 0 ORDER BY id LIMIT 10;
-- 下一页（记住上一页最后一条的 id）
SELECT * FROM t WHERE id > last_id ORDER BY id LIMIT 10;
```

要求 id 有序且连续（或至少单调递增），每次只扫 10 行。

## 分库分表

### 水平拆分

数据分散到多个结构相同的表/库（按 hash 或范围），提升并发和容量。

- 单表建议上限 500 万~1000 万行
- 路由方式：hash 取模、范围分区、一致性哈希
- 中间件：ShardingSphere、MyCat、Vitess

### 垂直拆分

按列拆分，高频字段和低频字段分离，提高缓存命中率。

- 将 TEXT/BLOB 等大字段拆到独立表
- 将访问频率差异大的字段分开
