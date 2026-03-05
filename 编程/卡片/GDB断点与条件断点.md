---
tags:
  - GDB
up:
  - "[[编程/归档/八股文/GDB]]"
down:
relation:
---

断点是 GDB 调试的核心功能，让程序在指定位置暂停执行。

## 断点类型

| 类型 | 命令 | 说明 |
|------|------|------|
| 普通断点 | `break func` / `b file:line` | 在函数入口或指定行暂停 |
| 临时断点 | `tbreak func` | 触发一次后自动删除 |
| 条件断点 | `break line if expr` | 仅当条件为 true 时暂停 |
| 忽略断点 | `ignore N count` | 断点 N 跳过前 count 次 |

## 条件断点

在循环中定位特定迭代特别有用：

```gdb
break 42 if i == 100          # 第 42 行，仅当 i==100 时停
break func if ptr == 0        # 空指针时停
condition 3 x > 10            # 给已存在的 3 号断点添加条件
condition 3                   # 取消 3 号断点的条件
```

## 断点管理

| 命令 | 说明 |
|------|------|
| `info breakpoints` / `info b` | 列出所有断点（编号、位置、条件、命中次数） |
| `delete N` | 删除编号为 N 的断点 |
| `delete` | 删除所有断点 |
| `disable N` | 临时禁用断点 N（不删除） |
| `enable N` | 重新启用断点 N |
| `clear` | 删除当前行的所有断点 |
| `clear func` | 删除函数入口的断点 |

## 断点命中时自动执行

```gdb
break 42
commands 1          # 为 1 号断点设置自动命令
  print x
  print y
  continue           # 打印后自动继续执行
end
```

适用场景：在不修改代码的情况下添加"日志输出"。

## 硬件断点

对只读内存或 ROM 代码使用硬件断点：
```gdb
hbreak address       # 使用 CPU 硬件调试寄存器
```
数量受限（通常 4 个），但不修改指令内存。
