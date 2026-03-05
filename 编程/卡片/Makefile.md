---
tags:
  - Linux
up:
  - "[[Linux]]"
down:
relation:
  - "[[静态链接和动态链接的区别]]"
  - "[[程序编译流程]]"
---
# Makefile

[![](https://cdn.nlark.com/yuque/0/2023/png/25992891/1698673489182-4767f9df-1a60-4f28-8d86-de30f4fe72a0.png)](https://cdn.nlark.com/yuque/0/2023/png/25992891/1698673489182-4767f9df-1a60-4f28-8d86-de30f4fe72a0.png)

### 编译器与C++编译

[[静态链接和动态链接的区别]]

### 编译过程

[[程序编译流程]]

### Make

[![](https://cdn.nlark.com/yuque/0/2023/png/25992891/1698673724601-96e523f7-5130-40ac-a5e6-362ef3348041.png)](https://cdn.nlark.com/yuque/0/2023/png/25992891/1698673724601-96e523f7-5130-40ac-a5e6-362ef3348041.png)

[![](https://cdn.nlark.com/yuque/0/2023/png/25992891/1698673764529-6a89069e-952b-4d86-89ba-a7416bc30f52.png)](https://cdn.nlark.com/yuque/0/2023/png/25992891/1698673764529-6a89069e-952b-4d86-89ba-a7416bc30f52.png)

- $@ 是目标变量
- $^是所有依赖文件
- $< 是第一个依赖文件
- $(wildcard *.c)获取所有.c文件
- $(error):传递error信息并结束运行make
- $(warning)：传递warning信息，继续运行make
- 可以使用通配符：* ? ~
- $(patsubst %.c,%.o,$(wildcard *.c))，首先使用“wildcard”函数获取工作目录下的.c文件列表；之后将列表中所有文件名的后缀.c替换为.o。这样我们就可以得到在当前目录可生成的.o文件列表
- 使用.PHONE指定伪目标
