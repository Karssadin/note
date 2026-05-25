---
tags:
  - 编程
  - 操作系统
  - Linux
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
  - "[[Linux下Profile和bashrc的区别]]"
  - "[[Makefile]]"
  - "[[Shell编程基础]]"
  - "[[Linux进程管理]]"
  - "[[crontab定时任务]]"
  - "[[Socket编程]]"
relation:
  - "[[Git]]"
  - "[[操作系统基础]]"
  - "[[计算机网络]]"
  - "[[Go工程化]]"
---
# Linux 基础

## 概述
- Linux 是开源的多用户、多任务操作系统，一切皆文件
- 主要发行版：Ubuntu、CentOS、Debian、Arch

## 文件系统
1. [[Linux目录结构]]
2. [[Linux下Profile和bashrc的区别]]
3. 常见文件类型：普通文件 `-`、目录 `d`、链接 `l`、块设备 `b`、字符设备 `c`、管道 `p`、套接字 `s`
4. 文件权限：`rwx`（读/写/执行），`chmod`、`chown`、`chgrp` 管理权限

## 基本命令
1. [[Linux指令与选项 格式]]
2. [[Linux常用指令]]（含文件目录、文本处理、权限、进程、网络、磁盘、压缩、重定向）
3. 文件与目录：`ls`、`cp`、`mv`、`rm`、`touch`、`mkdir`、`cd`、`pwd`、`tree`
5. 查找与过滤：`find`、`grep`、`awk`、`sed`
6. 网络：`ping`、`ifconfig`/`ip`、`netstat`/`ss`、`ssh`、`scp`、`curl`
7. 包管理
    - [[apt update]]
    - Debian 系：`apt install/remove/upgrade`
    - Red Hat 系：`yum`/`dnf`

## 文本编辑
1. [[Linux文件编辑-vim、gedit、nano]]

# 开发工具与环境

## 编译与构建
1. [[GCC、G++、GDB、CMake安装]]
2. [[GCC、G++]]
3. [[Makefile]]

## 调试
1. [[GDB]]

## 版本控制
1. [[Git]]

## 编辑器与 IDE
1. Vim：Linux 标准编辑器，模式操作（普通/插入/命令）

# [[Shell编程基础]]

详见 [[Shell编程基础]]，包含：变量、特殊变量、条件判断、循环、函数、管道与重定向、脚本调试

# 系统编程

## 进程管理
1. [[fork]]：进程创建
2. [[进程间通信的基本概念和方法]]
    1. [[管道]]、[[命名管道]]
    2. [[消息队列]]
    3. [[共享内存]]
    4. [[信号]]

## 线程管理
1. pthread 库：`pthread_create`、`pthread_join`、`pthread_detach`
2. 线程同步：[[互斥锁]]、[[条件变量]]、[[信号量]]
3. [[死锁的概念及产生原因]]

## 文件与 I/O
1. 文件描述符：`open`、`read`、`write`、`close`、`lseek`
2. 高效 I/O：`mmap`（内存映射）、[[IO多路复用及其技术（select、poll、epoll）]]

# 系统管理

## [[Linux进程管理]]

详见 [[Linux进程管理]]，包含：ps/top/htop、kill/nohup/jobs、systemd 服务管理、进程优先级

## [[crontab定时任务]]

详见 [[crontab定时任务]]，包含：crontab 命令、时间格式、常用示例

## 日志
- 系统日志：`/var/log/syslog`、`/var/log/messages`
- journald：`journalctl -u <service> -f`（跟踪服务日志）
- 应用日志：`tail -f /path/app.log`

## 磁盘与存储
- `df -h`：文件系统磁盘使用
- `du -sh <dir>`：目录大小
- `mount`/`umount`：挂载/卸载
- `fdisk`、`lsblk`：分区管理

## 用户与权限
- `useradd`、`usermod`、`userdel`：用户管理
- `passwd`：修改密码
- `sudo`：临时提权
- `su`：切换用户

# 网络

## [[Socket编程]]

详见 [[Socket编程]]，包含：套接字类型、TCP 编程流程、核心 API、地址复用、非阻塞 I/O

## 网络工具
- `ss -tlnp`：查看监听端口（替代 netstat）
- `iptables` / `firewalld`：防火墙配置
- `tcpdump`：抓包分析
- `nc`（netcat）：网络调试

# 安全

- 文件权限：`chmod 755`、`chown user:group file`
- SSH：`ssh-keygen` 生成密钥、`~/.ssh/config` 配置别名
- 加密库：OpenSSL
