---
tags:
  - 操作系统
up:
  - "[[IO与进程间通信]]"
down:
relation:
  - "[[阻塞IO与非阻塞IO]]"
  - "[[日志]]"
---

## 概念定义

- **同步 IO**：发起 IO 操作后，调用者**必须等待** IO 完成才能继续执行（无论是阻塞还是轮询）
- **异步 IO**：发起 IO 操作后，调用者**立即返回**，继续执行其他逻辑；IO 完成后通过**回调/事件通知**

> 注意：同步/异步描述的是**结果通知方式**，阻塞/非阻塞描述的是**等待期间线程状态**

## 四种 IO 模型

| IO 模型 | 同步/异步 | 阻塞/非阻塞 | 说明 |
|---------|---------|------------|------|
| 阻塞 IO | 同步 | 阻塞 | `read()` 时线程挂起，等数据就绪 |
| 非阻塞 IO | 同步 | 非阻塞 | `read()` 立即返回 EAGAIN，轮询等就绪 |
| IO 多路复用 | 同步 | 阻塞（select/poll/epoll 调用处） | 监听多 fd，就绪后同步读取 |
| 异步 IO（AIO） | 异步 | 非阻塞 | 注册回调，内核完成后通知进程 |

## Linux 异步 IO

### 传统 POSIX AIO（glibc aio_read）
- 用线程模拟异步，性能有限，实际较少使用

### Linux 内核 AIO（`io_submit`）
- 真正的内核异步，配合 Direct IO 使用
- 不经过 page cache，适合数据库日志写入（WAL）

### io_uring（Linux 5.1+）
- 通过共享环形队列避免系统调用开销
- 同时支持网络 IO 和文件 IO，性能极高

```cpp
// io_uring 简单使用示例
io_uring ring;
io_uring_queue_init(32, &ring, 0);

io_uring_sqe* sqe = io_uring_get_sqe(&ring);
io_uring_prep_read(sqe, fd, buf, sizeof(buf), 0);
io_uring_submit(&ring);

io_uring_cqe* cqe;
io_uring_wait_cqe(&ring, &cqe);  // 等待完成
io_uring_cqe_seen(&ring, cqe);
```

## 与数据库的关联

- MySQL redo log 刷盘策略（`innodb_flush_log_at_trx_commit`）：
  - `=1`：每次提交调用 `fsync()`（同步 IO，最安全）
  - `=2`：写 OS 缓冲区，每秒 `fsync()`（折中）
  - `=0`：仅写内存，每秒刷盘（性能最好，最不安全）
