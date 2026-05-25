---
tags:
  - GDB
up:
  - "[[GDB]]"
down:
relation:
  - "[[core dump调试]]"
  - "[[GDB断点与条件断点]]"
---

# GDB栈帧分析

栈帧分析用于定位函数调用路径、参数值和局部变量状态，是 core dump 和运行时调试的基础。

## 常用命令

```gdb
bt
bt full
frame <n>
up
down
info args
info locals
```

## 排查思路

1. 用 `bt` 找到崩溃或异常路径。
2. 用 `frame` 切换到可疑函数。
3. 用 `info args` 和 `info locals` 查看参数和局部变量。
4. 对优化编译产生的 `<optimized out>`，尝试使用带调试信息且降低优化级别的构建。
