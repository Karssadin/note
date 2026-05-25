---
tags:
  - GDB
up:
  - "[[GDB]]"
down:
relation:
  - "[[GDB断点与条件断点]]"
  - "[[线程与进程的区别]]"
---

# GDB多线程调试

GDB 可以查看和切换线程，适合排查死锁、竞态和线程卡住的问题。

## 常用命令

```gdb
info threads
thread <id>
thread apply all bt
set scheduler-locking on
set scheduler-locking off
```

## 说明

1. `info threads` 查看所有线程。
2. `thread <id>` 切换当前线程。
3. `thread apply all bt` 打印所有线程调用栈。
4. `scheduler-locking on` 可让单步调试时只运行当前线程。
