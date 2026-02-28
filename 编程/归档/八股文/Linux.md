---
tags:
  - 编程
  - 操作系统
up: 
down:
  - "[[GCC、G++、GDB、CMake安装]]"
  - "[[Linux常用指令]]"
  - "[[GCC、G++]]"
  - "[[死锁的概念及产生原因]]"
  - "[[apt update]]"
  - "[[Linux目录结构]]"
  - "[[Linux文件编辑-vim、gedit、nano]]"
  - "[[Linux指令与选项 格式]]"
relation:
  - "[[Linux下使用VS Code进行C++开发]]"
  - "[[Git]]"
  - "[[Makefile]]"
  - "[[操作系统基础]]"
---
# Linux 基础

- Linux概述
    - 历史背景
    - 主要发行版
    - Linux是**开源的操作系统**
    - Linux属于**多用户、多任务**：
        - 单用户：一个用户，在登录计算机(操作系统)，只能允许同时登录一个用户
        - 单任务：一个任务，允许用户同时进行的操作任务数量
        - 多用户：多个用户，在登录计算机(操作系统)，允许同时登录多个用户进行操作
        - 多任务：多个任务，允许用户同时进行多个操作任务
    - Linux一切皆文件：创建、编辑、保存、关闭、重命名、删除、恢复
- 文件系统
    - 目录结构
        - [[Linux目录结构]]
        - [[Linux下Porfile和bashrc的区别]]
    - 常见文件类型
    - 文件权限与属性
- 基本命令
    - [[Linux指令与选项 格式]]
    - [[Linux常用指令]]
    - [[Linux常用命令]]
    - 文件操作命令（ls, cp, mv, rm, touch, mkdir, etc.）
        - [[Linux文件编辑-vim、gedit、nano]]
    - 目录操作命令（cd, pwd, tree）
    - 权限操作命令（chmod, chown, chgrp）
    - 查找与过滤命令（find, grep, awk, sed）
    - 网络操作命令（ping, ifconfig, netstat, ssh）
    - 包管理命令
        - [[apt update]]

# 开发工具与环境

- 版本控制
    - [[Git]]
- 编辑器与 IDE
    - Vim, Emacs
    - [[Linux下使用VS Code进行C++开发]]
    - CLion

# C/C++编程

- 编译与链接
    - [[GCC、G++、GDB、CMake安装]]
    - [[GCC、G++]]
    - [[Makefile]]
- 常用库
    - 标准库（libc, libstdc++）
    - 第三方库（Boost, GLib）
- 调试与优化
    - [[GDB]]
    - 性能分析工具（perf, valgrind）

# Shell 编程

- Shell 概述
    - 常见 Shell 类型（bash, zsh, sh, etc.）
- 脚本基础
    - 脚本编写与执行
    - 基本语法与结构
    - 变量与环境变量
- 控制结构
    - 条件判断（if, else, elif）
    - 循环结构（for, while, until）
- 函数与脚本调试
    - 函数定义与调用
    - 脚本调试技术（set -x, set -e）

  

# 系统编程

- 进程管理
    - 进程创建（fork, exec）
    - 进程通信（管道, 消息队列, 共享内存, 信号）
- 线程管理
    - pthread 库
    - 线程同步（互斥锁, 条件变量, 信号量）
    - [[死锁的概念及产生原因]]
- 文件与 I/O 操作
    - 文件描述符与操作（open, read, write, close）
    - 高效 I/O（mmap, select, poll, epoll）

# 内核编程

- 内核模块
    - 模块编写与加载
    - 内核模块通信（netlink, proc 文件系统）
- 内核同步机制
    - 原子操作
    - 自旋锁与信号量
- 内核调试与分析
    - printk 日志
    - 调试工具（kgdb, ftrace, kprobe）

# 网络编程

- 套接字编程
    - 套接字类型（TCP, UDP）
    - 套接字 API 使用（socket, bind, listen, accept, connect, send, recv）
- 高级网络编程
    - 非阻塞 I/O 与多路复用（select, poll, epoll）
    - 网络协议实现（HTTP, FTP, DNS）

# 安全与加密

- 系统安全
    - 用户与权限管理
    - 防火墙配置（iptables, firewalld）
- 加密技术
    - 对称加密与非对称加密
    - 常用加密库（OpenSSL, GnuTLS）

# 数据库操作

- 数据库安装与配置
    - MySQL, PostgreSQL, SQLite
- 数据库访问编程
    - SQL 基本操作
    - 数据库连接与查询（libmysqlclient, libpq）

# 自动化与运维

- 配置管理工具
    - Ansible, Puppet, Chef
- 容器与虚拟化
    - Docker 使用
    - KVM/QEMU 配置