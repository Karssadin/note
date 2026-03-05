---
tags:
  - Linux
up:
  - "[[编程/归档/八股文/Linux]]"
down:
relation:
  - "[[IO多路复用及其技术（select、poll、epoll）]]"
---

Socket（套接字）是网络编程的基础抽象，提供进程间网络通信的接口。

## Socket 类型

| 类型 | 宏 | 协议 | 特点 |
|------|-----|------|------|
| 流套接字 | SOCK_STREAM | TCP | 可靠、有序、面向连接 |
| 数据报套接字 | SOCK_DGRAM | UDP | 不可靠、无连接、有边界 |
| 原始套接字 | SOCK_RAW | IP | 直接操作 IP 层 |

## TCP 编程流程

```
服务端                         客户端
socket()                      socket()
  ↓                              ↓
bind()                           │
  ↓                              │
listen()                         │
  ↓                              ↓
accept() ◄──── 三次握手 ────► connect()
  ↓                              ↓
read/write ◄─── 数据传输 ──► read/write
  ↓                              ↓
close() ◄──── 四次挥手 ────► close()
```

## 核心 API

```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);  // 创建套接字

struct sockaddr_in addr;
addr.sin_family = AF_INET;
addr.sin_port = htons(8080);
addr.sin_addr.s_addr = INADDR_ANY;

bind(sockfd, (struct sockaddr*)&addr, sizeof(addr));  // 绑定地址
listen(sockfd, 128);           // 监听，backlog 为半连接队列大小
int connfd = accept(sockfd, NULL, NULL);  // 阻塞等待客户端连接

// 客户端
connect(sockfd, (struct sockaddr*)&addr, sizeof(addr));

// 数据传输
ssize_t n = read(connfd, buf, sizeof(buf));
write(connfd, msg, strlen(msg));

close(connfd);
close(sockfd);
```

## 常见问题

- **地址复用**：`setsockopt(fd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt))`，避免 TIME_WAIT 导致 bind 失败
- **非阻塞 I/O**：`fcntl(fd, F_SETFL, O_NONBLOCK)`
- **高并发**：单线程 + [[IO多路复用及其技术（select、poll、epoll）]]，或多线程/多进程模型
