---
tags:
  - 编程
  - 八股文
  - 数据库
  - MySQL
up:
down:
  - "[[基础概念]]"
  - "[[日志]]"
  - "[[事物]]"
  - "[[索引]]"
  - "[[锁与并发控制]]"
  - "[[Buffer Pool]]"
  - "[[主从复制与高可用]]"
  - "[[MySQL性能优化]]"
  - "[[InnoDB存储引擎]]"
  - "[[MySQL存储引擎的区别]]"
relation:
  - "[[CMU15445]]"
---

# [[基础概念]]

详见 [[基础概念]]，包含：三大范式、MySQL 整体框架、select 语句执行流程、存储引擎对比、SQL 约束、数据类型与操作

相关卡片：[[InnoDB存储引擎]]、[[MySQL存储引擎的区别]]、[[数据库存储方式]]

# [[索引]]

详见 [[索引]]，包含：
1. [[索引概念]]、[[引入索引原因]]、[[索引缺点]]
2. [[索引创建时机？创建-避免创建]]、[[创建索引语句]]
3. [[MySQL索引类]]：聚簇索引、二级索引、覆盖索引、联合索引、前缀索引
4. B+ 树：为什么选择 B+ 树而非 B 树 / 红黑树 / 哈希
5. 索引下推、索引失效场景、最左前缀匹配原则
6. `count(*)` vs `count(1)` vs `count(列)`

# [[事物|事务]]

详见 [[事物|事务]]，包含：
1. 事务定义、ACID 特性与 InnoDB 实现
2. 隔离级别：读未提交 / 读提交 / 可重复读 / 串行化
3. 脏读、不可重复读、幻读
4. MVCC：Read View、版本链、可见性判断
5. 两阶段提交：redo log prepare → binlog → redo log commit

# [[锁与并发控制]]

详见 [[锁与并发控制]]，包含：
1. 全局锁、表级锁（共享锁/排他锁/元数据锁/意向锁/AUTO-INC锁）
2. 行级锁：Record Lock、Gap Lock、Next-Key Lock、插入意向锁
3. 乐观锁与悲观锁
4. 死锁：[[死锁的概念及产生原因]]、检测与预防
5. 行级锁加锁方式与具体场景
6. update 没加索引为什么会锁全表

# [[日志]]

详见 [[日志]]，包含：
1. redo log：WAL 机制、循环写入、刷盘策略（`innodb_flush_log_at_trx_commit`）
2. undo log：事务回滚、MVCC 版本链
3. binlog：三种格式（STATEMENT/ROW/MIXED）、刷盘策略（`sync_binlog`）
4. 三大日志对比（层级、类型、作用、写入时机）
5. 两阶段提交详解、组提交优化
6. 主从复制原理（binlog → relay log → 重放）

# [[Buffer Pool]]

详见 [[Buffer Pool]]，包含：
1. 缓冲池概念、缓存内容（数据页/索引页/Undo 页）
2. 页面管理：Free List、LRU List（young/old 分区）、Flush List
3. 脏页刷盘与抖动、预读机制

# [[主从复制与高可用]]

详见 [[主从复制与高可用]]，包含：
1. 主从复制原理（binlog → relay log → 重放）
2. 复制模式：异步 / 半同步 / 同步
3. 读写分离：应用层路由 / 中间件（ProxySQL、MyCat）
4. 高可用方案：MHA、Group Replication、InnoDB Cluster

# [[MySQL性能优化]]

详见 [[MySQL性能优化]]，包含：
1. 慢查询定位与 Explain 分析
2. SQL 优化要点
3. JOIN 优化（NLJ / BNL）
4. 分页优化（延迟关联 / 游标分页）
5. 分库分表（水平拆分 / 垂直拆分）
