---
tags:
  - 操作系统
  - Linux
up:
  - "[[IO与进程间通信]]"
down:
relation:
  - "[[Socket编程]]"
  - "[[阻塞IO与非阻塞IO]]"
  - "[[同步IO与异步IO]]"
  - "[[协程]]"
  - "[[进程间通信的基本概念和方法]]"
  - "[[Go netpoll]]"
---

**I/O 多路复用**：用**单个线程**同时监视多个文件描述符（fd），当某个 fd 就绪时才进行读写，避免为每个连接创建线程的高开销。

## 三种技术对比

| 特性 | `select` | `poll` | `epoll` |
|------|---------|--------|---------|
| fd 数量限制 | 1024（FD_SETSIZE） | 无限制 | 无限制 |
| 时间复杂度 | O(n)，每次遍历所有 fd | O(n)，每次遍历所有 fd | O(1)（就绪事件通知） |
| 内核实现 | 轮询 | 轮询 | 回调（红黑树+就绪链表） |
| 跨平台 | ✅ POSIX 标准 | ✅ POSIX 标准 | ❌ 仅 Linux |
| 重置问题 | 每次调用需重置 fd_set | 不需要 | 不需要 |
| 触发模式 | 仅水平触发 LT | 仅水平触发 LT | 支持 LT 和 ET |

## 1. select

```cpp
#include <sys/select.h>

fd_set read_fds;
FD_ZERO(&read_fds);
FD_SET(sockfd, &read_fds);

struct timeval timeout = {5, 0};  // 5秒超时
int ready = select(sockfd + 1, &read_fds, nullptr, nullptr, &timeout);

if (ready > 0 && FD_ISSET(sockfd, &read_fds)) {
    // sockfd 有数据可读
    read(sockfd, buf, sizeof(buf));
}
```

**缺点**：fd_set 每次调用前都要重置；最大 1024 个 fd；O(n) 遍历。

## 2. poll

```cpp
#include <poll.h>

struct pollfd fds[MAX_FDS];
fds[0].fd = sockfd;
fds[0].events = POLLIN;   // 监听可读事件

int ready = poll(fds, nfds, 5000);  // 5000ms 超时

for (int i = 0; i < nfds; i++) {
    if (fds[i].revents & POLLIN) {
        // fds[i].fd 有数据可读
    }
}
```

**改进**：无 fd 数量限制，不需要重置集合；但仍需 O(n) 遍历。

## 3. epoll（推荐，高并发标准方案）

```cpp
#include <sys/epoll.h>

// 1. 创建 epoll 实例（内核中维护红黑树）
int epfd = epoll_create1(0);

// 2. 注册/修改/删除感兴趣的 fd
struct epoll_event ev;
ev.events = EPOLLIN | EPOLLET;  // 可读 + 边缘触发
ev.data.fd = sockfd;
epoll_ctl(epfd, EPOLL_CTL_ADD, sockfd, &ev);  // 添加
// epoll_ctl(epfd, EPOLL_CTL_MOD, sockfd, &ev);  // 修改
// epoll_ctl(epfd, EPOLL_CTL_DEL, sockfd, nullptr); // 删除

// 3. 等待事件（只返回就绪的 fd）
struct epoll_event events[MAX_EVENTS];
while (true) {
    int nready = epoll_wait(epfd, events, MAX_EVENTS, -1);  // -1: 无限等待
    for (int i = 0; i < nready; i++) {
        if (events[i].events & EPOLLIN) {
            int fd = events[i].data.fd;
            // 处理可读事件
        }
    }
}
close(epfd);
```

## 水平触发（LT）vs 边缘触发（ET）

| 触发模式 | 行为 | 适用场景 |
|---------|------|---------|
| **LT（Level Triggered）** | 只要 fd 就绪，每次 `epoll_wait` 都通知 | 默认，编程简单，适合大部分场景 |
| **ET（Edge Triggered）** | fd 状态**变化**时才通知一次 | 高性能场景，必须一次性读完所有数据 |

**ET 模式注意事项**：
- 必须将 fd 设置为**非阻塞**（`O_NONBLOCK`），否则 `read` 可能永久阻塞
- 必须循环读到 `EAGAIN` 为止，确保数据读完

```cpp
// ET 模式下读取所有数据
if (events[i].events & EPOLLIN) {
    char buf[4096];
    ssize_t n;
    while ((n = read(fd, buf, sizeof(buf))) > 0) {
        // 处理数据
    }
    if (n == -1 && errno != EAGAIN) {
        // 真正的错误
    }
    // n == 0：连接关闭
}
```

## epoll 的内核实现原理

1. `epoll_create`：在内核创建一个事件对象（含**红黑树** + **就绪链表**）
2. `epoll_ctl(ADD)`：将 fd 注册到红黑树，并设置**回调函数**（fd 就绪时触发）
3. 当 fd 就绪（如网卡收到数据）：内核回调将该 fd 加入**就绪链表**
4. `epoll_wait`：只需遍历就绪链表，返回就绪的 fd，O(1) 时间

## epoll 服务器示例（Reactor 模式）

```cpp
// 简化版 Reactor：epoll 驱动事件分发
int listenfd = ...; // 已绑定的监听 socket
setNonBlocking(listenfd);

int epfd = epoll_create1(0);
addToEpoll(epfd, listenfd, EPOLLIN);

while (true) {
    int n = epoll_wait(epfd, events, MAX, -1);
    for (int i = 0; i < n; i++) {
        if (events[i].data.fd == listenfd) {
            // 新连接到来
            int connfd = accept(listenfd, ...);
            setNonBlocking(connfd);
            addToEpoll(epfd, connfd, EPOLLIN | EPOLLET);
        } else {
            // 已连接 fd 有数据
            handleRead(events[i].data.fd);
        }
    }
}
```

## 与协程/异步框架的关系

epoll 是现代高并发 C++ 框架的基础：
- **libuv**（Node.js 底层）：封装 epoll/kqueue
- **Boost.Asio**：跨平台异步 IO，底层用 epoll
- **C++20 协程 + epoll**：协程挂起等待 IO 就绪，恢复后继续执行（实现异步编程的同步风格）
