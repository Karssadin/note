---
tags:
  - GDB
up:
  - "[[编程/归档/八股文/GDB]]"
down:
relation:
---

Core dump（核心转储）是程序异常终止时操作系统自动生成的内存快照文件，包含崩溃时的进程内存、寄存器状态、调用栈等信息。

## 启用 core dump

默认可能被禁用（`ulimit -c` 为 0），需手动开启：

```bash
ulimit -c unlimited        # 当前 Shell 生效，不限制 core 文件大小
# 永久生效：编辑 /etc/security/limits.conf
# * soft core unlimited
```

### core 文件位置

```bash
cat /proc/sys/kernel/core_pattern
# 默认通常为 "core" 或 "/tmp/core.%e.%p"
# 设置格式：echo "/tmp/core.%e.%p.%t" > /proc/sys/kernel/core_pattern
# %e=程序名, %p=PID, %t=时间戳
```

## 使用 GDB 分析 core dump

```bash
gdb ./program core          # 加载程序和 core 文件
```

进入 GDB 后常用命令：

| 命令 | 说明 |
|------|------|
| `bt` / `backtrace` | 查看崩溃时的调用栈（最重要） |
| `frame N` | 切换到第 N 层栈帧 |
| `info locals` | 查看当前栈帧的局部变量 |
| `info registers` | 查看寄存器状态 |
| `print var` | 查看变量值 |
| `list` | 查看崩溃位置的源码 |

## 典型排查流程

1. 编译时加 `-g` 保留调试符号
2. 运行程序触发崩溃 → 生成 core 文件
3. `gdb ./program core` → `bt` 查看调用栈
4. 定位崩溃的函数和行号 → `frame N` 切换到对应栈帧
5. `info locals` + `print` 查看崩溃时的变量状态
6. 根据变量状态判断 bug 原因（空指针、越界、double free 等）

## 常见崩溃信号

| 信号 | 原因 |
|------|------|
| SIGSEGV (11) | 段错误：访问非法内存地址 |
| SIGABRT (6) | abort()：assert 失败或 double free |
| SIGFPE (8) | 浮点异常：除以零 |
| SIGBUS (7) | 总线错误：非对齐内存访问 |
