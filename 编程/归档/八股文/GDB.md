---
tags:
  - 编程
  - 八股文
  - GDB
up:
down:
  - "[[GCC、G++、GDB、CMake安装]]"
  - "[[core dump调试]]"
  - "[[GDB断点与条件断点]]"
  - "[[GDB内存检查]]"
relation:
  - "[[Linux]]"
---
- [[#常用技巧与配置|常用技巧与配置]]
- [[#启动和退出|启动和退出]]（含 [[core dump调试]]）
- [[#程序执行控制|程序执行控制]]
- [[#断点操作|断点操作]]（详见 [[GDB断点与条件断点]]）
- [[#查看变量|查看变量]]（详见 [[GDB内存检查]]）
- [[#查看栈帧|查看栈帧]]
- [[#多线程调试|多线程调试]]
- [[#TUI模式|TUI模式]]
- [[#代码实践|代码实践]]

## 常用技巧与配置

> 回车键表示重复上一个命令

### 编译
- 编译时需要添加 `-g` 选项生成调试符号：`g++ -g main.cpp -o main`
- `-O0` 禁止优化，避免变量被优化掉

### 日志输出
- `set logging on` — 启用日志记录
- `set logging file gdb.log` — 设置日志文件名
- `set logging off`

### 信号处理
- `handle SIGUSR1 nostop noprint` — 忽略指定信号

### ~/.gdbinit 默认配置

```shell
set history save on
set print elements 0    # 数据输出全部，不截断
set print pretty on
set print array on
```

## 启动和退出

| 命令 | 说明 |
|------|------|
| `gdb program` | 启动 GDB 并加载程序 |
| `gdb program core` | 加载 core dump 文件 |
| `gdb -p PID` | attach 到运行中的进程 |
| `gdb -q program` | 安静模式，不显示版本信息 |
| `quit` / `q` | 退出 GDB |

## 程序执行控制

| 命令 | 说明 |
|------|------|
| `run` / `r` | 启动程序（可加参数：`r --flag`） |
| `continue` / `c` | 从断点处继续执行 |
| `next` / `n` | 执行下一行（跳过函数调用） |
| `step` / `s` | 单步执行（进入函数内部） |
| `finish` | 执行到当前函数返回 |
| `until` / `u` | 执行到指定行或循环结束 |
| `jump location` | 跳转到指定行号执行 |

- 宕机后使用 `r --gdb` 重新启动，断点仍然保留

## 断点操作

| 命令 | 说明 |
|------|------|
| `break func` / `b func` | 在函数入口设置断点 |
| `break file:line` / `b file:line` | 在指定行设置断点 |
| `tbreak` | 临时断点（触发一次后自动删除） |
| `condition N expr` | 给断点 N 添加条件 |
| `info breakpoints` / `info b` | 列出所有断点 |
| `delete N` | 删除编号为 N 的断点 |
| `disable N` / `enable N` | 禁用/启用断点 |
| `clear` | 删除所有断点 |

## 查看变量

| 命令 | 说明 |
|------|------|
| `print expr` / `p expr` | 打印表达式的值 |
| `display var` | 每次停止时自动显示变量值 |
| `undisplay N` | 取消自动显示 |
| `whatis var` | 显示变量的类型 |
| `info locals` | 显示当前栈帧的局部变量 |
| `info args` | 显示当前函数的参数 |
| `set variable var=value` | 修改变量的值 |
| `call func(args)` | 调用函数并显示返回值 |
| `list` / `l` | 显示源代码 |

### 查看内存内容
- `x/nfu address` — 查看内存内容（n=数量，f=格式，u=单位大小）
- `x/20xw address` — 显示 20 个单元，十六进制，每单元 4 字节
- `set *(int*)0xaddress = value` — 修改指定内存地址的值
- `x/4tb &addr` — 输出 4 字节的详细字节内容

### watch point
- `watch expr` — 表达式值变化时暂停
- `rwatch expr` — 表达式被读取时暂停
- `awatch expr` — 表达式被访问（读/写）时暂停
- `info watchpoints` — 显示所有观察点

## 查看栈帧

| 命令 | 说明 |
|------|------|
| `backtrace` / `bt` | 显示调用栈 |
| `frame N` / `f N` | 切换到第 N 层栈帧 |
| `info frame` / `i f` | 查看当前栈帧详情 |
| `thread apply all bt` | 查看所有线程的栈帧 |

## 多线程调试

| 命令 | 说明 |
|------|------|
| `info threads` | 列出所有线程 |
| `thread N` | 切换到线程 N |
| `thread apply all cmd` | 对所有线程执行命令 |
| `set scheduler-locking on` | 只运行当前线程，其他冻结 |
| `set scheduler-locking off` | 恢复所有线程并行 |

- 多线程程序建议编译时加 `-lpthread`
- 使用 `thread apply all bt` 快速定位死锁

## TUI模式

- `Ctrl+X A` — 启用/退出 TUI 模式
- `layout src` — 源代码布局
- `layout asm` — 汇编代码布局
- `layout regs` — 寄存器布局
- `layout split` — 源代码 + 汇编分屏
- `focus src` / `focus cmd` — 切换焦点窗口

## 代码实践

```cpp
#include <iostream>
using namespace std;
int main(int argc, char** argv) {
    int N = 100;
    int sum = 0;
    int i = 1;
    while (i <= N) {
        sum = sum + i;
        i = i + 1;
    }
    cout << sum << endl;
    return 0;
}
```

- `g++ gdb_demo.cpp -g -o gdb_demo` 编译
- `break 10` 在第 10 行插入断点，`i b` 查看断点信息
- `run` 启动后在断点处停止
- `print i` 查看变量值，`display sum` 持续追踪
- `watch sum` 当 sum 变化时自动停止
