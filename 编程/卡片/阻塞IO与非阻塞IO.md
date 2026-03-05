---
tags:
  - 操作系统
up:
  - "[[IO与进程间通信]]"
down:
relation:
  - "[[同步IO与异步IO]]"
  - "[[IO多路复用及其技术]]"
---

## 概念

- **阻塞 IO**：调用 IO 函数时，如果数据未就绪，当前线程被**挂起**（让出 CPU），等待内核将数据准备好后才返回
- **非阻塞 IO**：调用 IO 函数时，如果数据未就绪，立即返回错误码 `EAGAIN`/`EWOULDBLOCK`，不挂起线程

## 系统调用级别

```cpp
// 将 fd 设置为非阻塞
int flags = fcntl(fd, F_GETFL, 0);
fcntl(fd, F_SETFL, flags | O_NONBLOCK);

// 非阻塞 read
ssize_t n = read(fd, buf, sizeof(buf));
if (n == -1 && errno == EAGAIN) {
    // 数据还没准备好，稍后重试
}
```

## 阻塞 vs 非阻塞对比

| 维度 | 阻塞 IO | 非阻塞 IO |
|------|---------|---------|
| 线程状态 | 挂起，让出 CPU | 持续运行，轮询 |
| CPU 利用率 | 等待期间不占 CPU | 轮询时消耗 CPU |
| 编程难度 | 简单 | 复杂（需处理 EAGAIN） |
| 适用场景 | 单连接或线程数不受限 | 配合 IO 多路复用使用 |

## 与 IO 多路复用的关系

非阻塞 IO 通常配合 `select`/`poll`/`epoll` 使用：
1. 用 epoll 监听多个 fd
2. epoll 通知某 fd 就绪
3. 对该 fd 执行非阻塞 read（保证不会阻塞）

```
数据就绪？
  └── 是 → read 立即返回数据（非阻塞）
  └── 否 → 返回 EAGAIN，epoll 继续等待（不阻塞线程）
```

## 套接字默认行为

- 默认情况下，socket 是**阻塞**的
- `accept()`、`connect()`、`read()`、`write()` 均会阻塞
- 高并发服务器通常设置 `O_NONBLOCK` + epoll ET 模式
