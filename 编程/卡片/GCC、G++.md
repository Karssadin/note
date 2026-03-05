---
tags:
  - Linux
up:
  - "[[Linux]]"
down:
relation:
  - "[[GCC、G++、GDB、CMake安装]]"
---
- 使用gcc指令编译C代码
- 使用g++指令编译C++代码

# 编译过程

1. 预处理`-E`：生成.i文件

`g++ -E test.cpp -o test.i`

1. 编译`-S`：生成.s文件

`g++ -S test.i -o test.s`

1. 汇编`-c` ：生成.o文件

`g++ -c test.s -o test.o`

1. 连接：生成可执行bin文件

`g++ test.o -o test`

- 通过上面四条指令与下面一个指令生成的文件一样

`g++ test.cpp -o test`

# g++常用编译参数

- -g：告诉编译器产生能被GDB使用的调试信息，以调试程序

`g++ -g test.cpp`

- -O[n]： 优化远大吗
    - -O0：不做优化
    - -O1：默认优化
    - -O2：常用，完成处理-O1的优化外，进行一些其他指令调整
    - -O3：包括循环展开和一些其他特性相关的优化工作
    - 编译速度慢，执行速度快
- -l、-L：指定库文件、指定库文件路径
    - 如果使用-l的话，后面紧跟库名，会在`LD_LIBRARY_PATH`路径中进行寻找，如果没有放在这里面路径中需要使用-L，环境变量默认有：/usr/lib 、 /lib 和 /usr/local/lib中
    - `g++ -L. -lmytestlig test.cpp -o test`
- -I：inclue，指定头文件搜索目录
    - /usr/include不需要指定，如果不在目录下，就需要指定了
    - `g++ -I. test.cpp`
- -Wall：打印警告信息
- -w：关闭警告信息
- -std=c++11：设置编译标准
- -o：指定输出文件名
- -D：定义宏
    
    - -DDEBUG，定义DEBUG宏，文件中有与该宏相关的代码，用DEBUG来选择开启或关闭
    
    ```C++
    \#include <iostream>
    
    int main()
    {
    	\#ifdef DEBUG
    		std::cout << "DEBUG LOG" << std::endl;
    	\#endif
    		std::cout << "😯" << std::endl;
    }
    //在编译的时候，使用g++ -DDEBUG main.cpp
    ```
    

  

# 样例

```C++
main.cpp
include/swap.h
src/swap.cpp
```

- 直接编译：`g++ main.cpp src/swap.cpp -Iinclude`
- 添加参数：`g++ main.cpp src/swap.cpp -Iinclude -Wall -std=c++11 -o main`
- 生成静态库：
    
    ```Shell
    cd src
    g++ swap.cpp -c -I../include
    ar rs libSwap.a swap.o
    # ar归档命令  r是更新，c创建 s建立索引
    cd ..
    g++ main.cpp -Iinclude -Lsrd -lSwap -o staticmain
    ```
    
- 生成动态库：
    
    ```Shell
    cd src
    g++ swap.cpp -I../include -fPIC -shared -o libSwap.so
    # g++ swap.cpp -I../include -c -fPIC
    # g++ -shared -olibSwap.so swap.o
    # -fPIC位置无关编码
    
    cd ..
    g++ main.cpp -Iinclude -Lsrc -lSwap -o sharedmain
    #动态库要运行的话，需要将环境变量中添加对应的路径
    # export LD_LIBRARY_PATH = $LD_LIBRARY_PATH : ./src
    ```
