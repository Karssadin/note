---
tags:
  - GDB
up:
  - "[[编程/归档/八股文/GDB]]"
down:
relation:
---

GDB 提供 `x` 命令检查内存内容，以及 watchpoint 监视数据变化。

## x 命令格式

```
x/nfu address
```

| 参数 | 含义 | 可选值 |
|------|------|--------|
| `n` | 显示单元数量 | 任意正整数 |
| `f` | 显示格式 | `x`(十六进制) `d`(十进制) `u`(无符号) `o`(八进制) `t`(二进制) `c`(字符) `s`(字符串) `i`(指令) |
| `u` | 单元大小 | `b`(1字节) `h`(2字节) `w`(4字节) `g`(8字节) |

### 示例

```gdb
x/20xw 0x7fffffffe000     # 从地址开始显示 20 个 4 字节单元，十六进制
x/10cb &str                # 显示 str 地址开始的 10 个字节，字符格式
x/4gx &value               # 显示 value 地址开始的 4 个 8 字节单元，十六进制
x/s &str                   # 显示以 \0 结尾的字符串
x/5i $pc                   # 显示当前 PC 开始的 5 条指令（反汇编）
```

### 修改内存

```gdb
set *(int*)0x7fff1234 = 42       # 修改指定地址的 4 字节整型值
set *(char*)0x7fff1234 = 'A'     # 修改单字节
```

## Watchpoint（数据断点）

监视变量或内存地址，当值发生变化时自动暂停。

| 类型 | 命令 | 触发条件 |
|------|------|---------|
| 写监视 | `watch expr` | 表达式的值被修改时 |
| 读监视 | `rwatch expr` | 表达式被读取时 |
| 读写监视 | `awatch expr` | 表达式被读取或修改时 |

```gdb
watch x                    # x 的值变化时停
watch *(int*)0x601040      # 监视特定内存地址
watch x if x > 100         # 条件 watchpoint
info watchpoints           # 查看所有 watchpoint
delete watch N             # 删除指定 watchpoint
```

### 性能注意

- 软件 watchpoint：GDB 单步执行每条指令并检查值，**非常慢**
- 硬件 watchpoint：使用 CPU 调试寄存器，几乎无开销，但数量有限（通常 4 个）
- 监视局部变量时，函数退出后 watchpoint 自动失效
