---
tags:
  - MySQL
up:
  - "[[数据库基础知识]]"
down:
relation:
  - "[[MySQL存储引擎的区别]]"
  - "[[Buffer Pool]]"
  - "[[索引]]"
  - "[[事务与并发控制]]"
---

InnoDB 是 MySQL 默认的事务型存储引擎（MySQL 5.5 起），支持 ACID 事务、行级锁和外键约束。

## 核心特性

1. **事务支持**：完整的 ACID 特性，通过 redo log 保证持久性，undo log 支持回滚
2. **行级锁**：并发性能优于表级锁，支持共享锁（S）和排他锁（X）
3. **MVCC**：多版本并发控制，读操作不阻塞写操作，通过 undo log 构建一致性视图
4. **聚簇索引**：数据按主键 B+ 树组织存储，主键查询效率高
5. **外键约束**：支持引用完整性检查
6. **崩溃恢复**：通过 redo log（WAL 机制）实现 crash-safe

## 存储结构

```
表空间 → 段（Segment）→ 区（Extent, 1MB）→ 页（Page, 16KB）→ 行（Row）
```

## Buffer Pool

InnoDB 使用 [[Buffer Pool]] 缓存数据页和索引页，采用改进的 LRU 算法管理页面淘汰。

## 日志系统

- **redo log**：物理日志，记录页的修改，保证事务持久性
- **undo log**：逻辑日志，记录行的旧版本，用于回滚和 MVCC
- **binlog**：MySQL Server 层日志，用于主从复制
