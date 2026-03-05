---
tags:
  - Linux
up:
  - "[[Linux]]"
down:
relation:
---
Linux 采用 FHS（Filesystem Hierarchy Standard）标准，一切皆文件，目录树从 `/`（根目录）展开。

## 核心目录

| 目录 | 全称 | 说明 |
|------|------|------|
| `/bin` | binary | 基本用户命令（ls、cp、cat），所有用户可用 |
| `/sbin` | super binary | 系统管理命令（fdisk、iptables），需 root 权限 |
| `/etc` | etcetera | 系统配置文件（nginx.conf、passwd、fstab） |
| `/home` | — | 普通用户家目录（`/home/username`） |
| `/root` | — | root 用户的家目录 |
| `/usr` | Unix System Resource | 用户安装的软件和库（`/usr/bin`、`/usr/lib`、`/usr/local`） |
| `/var` | variable | 可变数据：日志（`/var/log`）、缓存、邮件 |
| `/tmp` | temporary | 临时文件，重启可能清空 |
| `/dev` | device | 设备文件（硬盘 sda、终端 tty），需挂载后使用 |
| `/proc` | process | 虚拟文件系统，反映运行时内核和进程信息（`/proc/cpuinfo`） |
| `/mnt` | mount | 临时挂载点 |
| `/opt` | optional | 第三方软件安装目录 |
| `/lib` | library | 系统共享库（.so 文件） |
| `/boot` | — | 内核和引导加载程序（grub） |

## 常用路径

- `/usr/local/bin`：用户手动编译安装的程序
- `/etc/profile` 和 `~/.bashrc`：环境变量配置（详见 [[Linux下Porfile和bashrc的区别]]）
- `/var/log/syslog`：系统日志
- `/proc/meminfo`：内存信息
