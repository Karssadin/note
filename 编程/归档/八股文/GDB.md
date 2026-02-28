---
tags:
  - 编程
  - 八股文
up: 
down:
  - "[[GCC、G++、GDB、CMake安装]]"
relation:
  - "[[Linux下使用VS Code进行C++开发]]"
  - "[[Linux]]"
---
- [[#常用技巧|常用技巧]]
	- [[#常用技巧#输出日志到文件中|输出日志到文件中]]
	- [[#常用技巧#编译program|编译program]]
- [[#添加~/.gdbinit默认配置|添加~/.gdbinit默认配置]]
- [[#启动和退出|启动和退出]]
- [[#程序执行控制：|程序执行控制：]]
- [[#断点操作|断点操作]]
- [[#查看变量|查看变量]]
	- [[#查看变量#查看内存内容|查看内存内容]]
	- [[#查看变量#watch point|watch point]]
- [[#查看栈帧|查看栈帧]]
- [[#TUI模式|TUI模式]]
- [[#代码实践|代码实践]]
## 常用技巧
> 回车键，表示重复上一个命令
### 输出日志到文件中
- set logging on                  # 启用日志记录
- set logging file gdb.log        # 设置日志文件名
- set logging off
### 编译program
- 编译时需要添加`-g`选项
### 忽略kill信号
 - `handle SIGUSR1 nostop noprint`
### 添加~/.gdbinit默认配置

```shell
python
import glob
import sys

sys.path.insert(0, "/user/share/gcc-4.8.5/python")
from libstdcxx.v6.printers import register_libstdcxx_printers
register_libstdcxx+printers(None)

for f in glob.glob('/etc/gdbinti.d/*.gdb'):
	gdb.execute('source %s' % f)
for f in glob.glob('/etc/gdbinti.d/*.py'):
	gdb.execute('source %s' % f)
end
```

```shell
# /etc/gdbinit.d/config.gdb
define n
	next
	refresh
end
set history save on # history
set print elements 0  #数据输出全乎
set print pretty on
set print array on
```
## 启动和退出
- `gdb [program]                   # 启动 GDB 并加载指定程序 
- `gdb program core`             # 启动 `GDB`并加载`core dump`文件 
- `gdb program [PID]`            # 调试正在运行的进程 
- `gdb -q [program]`                # 安静模式启动，不显示版本信息  
- `quit 或 q`                       # 退出`GDB`调试器
## 程序执行控制：

- **run 或 r：启动程序。**
	- 启动之后需要输出 `r --gdb`来重新启动。
		- 如果遇到宕机之类的，使用`r --gdb`重新启动，断点还是会存在，不会消失
- continue 或 c：从断点处继续程序执行。
- next 或 n：执行下一行代码，跳过函数调用。
- step 或 s：单步执行，进入函数内部
- jump [loaction]：跳到指定行号
	- 假如说有判断，可能会提前导致程序结束，可以使用jump跳过判断，来查看不结束的执行情况
- fread(a, 8, f1_read, f1);
- finish：继续运行程序，直到当前函数完成为止。
## 断点操作

- **break [filename:]function 或 b [filename:]function：在指定函数上设置断点。**
- **break [filename:]linenumber 或 b [filename:]linenumber：在指定行号上设置断点。**
- **info breakpoints 或 info b：列出所有断点。**
- **delete [breakpoint_number]：删除指定编号的断点。**
- **clear：删除所有断点。**
- tbreak: 临时断点
## 查看变量

- print expression 或 p expr      # 打印表达式的值
- display variable                # 自动显示变量值，每次停止时更新
- undisplay num                   # 取消自动显示变量
- whatis variable                 # 显示变量的类型
- info locals                     # 显示当前栈帧的局部变量
- info args                       # 显示当前函数的所有参数
- set variable var=value          # 修改变量的值
- call function(args)             # 调用函数并显示返回值
- list 或 l：显示源代码。
	- 可以使用`show listsize` 查看当前会查询多少行的代码
	- 使用`set listsize`来设置`listsize`

### 查看内存内容
- `x/nfu address `                  # 查看内存内容
- `x/20xw address`                  # 显示地址处的 20 个单元，16 进制，每单元 4 字节
- `set *(int*)0xaddress = value`    # 修改指定内存地址的值
- 输出对应地址开始`n`字节的详细字节内容，而非用变量进行解析：`x/4tb &addr`
	- `4`表示取`4`字节，`addr`是起始地址
- 128 位 `SMID` 变量进行转换：`p *(int*)&add@4`
	- 4是位数，`int*`是类型
### watch point
- `watch expression`                # 当表达式的值发生变化时暂停执行
- `rwatch expression `              # 当表达式被读取时暂停执行
- `awatch expression`               # 当表达式被访问（读/写）时暂停执行
- `info watchpoints`              # 显示所有观察点
- `delete watch num`              # 删除指定观察点



## 查看栈帧
- backtrace 或 bt
- frame 或 f [数字] : 切换栈
- i f或 f 查看当前 栈
- t a a bt查看所有现成的栈帧


## TUI模式
- Ctrl + X 然后按 A # 启用/退出 TUI 模式 
- tui enable # 启用 TUI 模式 
- tui disable # 退出 TUI 模式
- layout src                      # 显示源代码布局
- layout asm                      # 显示汇编代码布局
- layout regs                     # 显示寄存器布局
- layout split                    # 显示源代码和汇编代码分屏布局
- fs s                            # 切换到源代码窗口
- fs c                            # 切换到命令窗口
- fs asm                          # 切换到汇编窗口
- fs regs                         # 切换到寄存器窗口



# 代码实践

```C++
\#include <iostream>
using namespace std;
int main(int argc, char ** argv)
{
    int N = 100;
    int sum = 0;
    int i = 1;
    while(i <= N)
    {
        sum = sum + i;
        i = i + 1;
    }
    cout << sum << endl;
    cout << "run over" << endl;

    return 0;
}
```


- `g++ gdb_demo.cpp -g -o gdb_demo`
- `run`之后直接执行结束
- 使用`break 10`在`10`行插入断点
    - 可以输入`i b` 查看断点信息
    - `run`之后在断点处停止
    - 输入`print i`可以查看`i`的变量值
    - 输入`p N`可以查看变量`N`的值
    - `continue`可以继续执行，还是会卡在`10`行，继续循环
    - 想查看`10`行周围的代码可以输入`list`或者`l`之后会输出`10`行上下代码

- 使用`display`追踪`sum`的值，可以输入`display sum`之后继续进行运行的时候会输出`sum`的值
- 使用`watch`可以追踪`sum`的值，使用`watch sum`之后当`sum`变化的时候会输出`sum`的值



