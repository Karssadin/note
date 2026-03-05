---
tags:
  - 编程
up:
down:
relation:
  - "[[C++]]"
---
# 工具介绍篇

## 创建简单工程

> chapter01/Chapter01_demo

- 编译：
	- debug、release
	- x86、x64
- 一个解决方案可以包含多个工程，这些工程可以是应用程序、可以是DLL、可以是.so

  

- 编译、链接过程和在一起了，可以分开
- 只编译的话，会生成.obj文件，而不会连接成exe

---

- 在生成
- 在项目上：生成、重新生成
- 在解决方案上：生成解决方案

  

# 工具配置

## 界面配置

- 蓝色
- 字体：C** Code
- 制表符替换为空格
- 显示空格、显示行数

  

- astyl可以格式化代码

## VA配置

- Visual Assist
    - Highlight
    - display： after column 80,80列放一个红线

  

# 工具使用篇（Tank-1990项目）

  

## 应用程序工程

- 简单应用建立

> Chapter02/executable/demo

- 多种模板区别
- 解决方案-工程
- 解决方案：Debug、release区别，x86/64（兼容性）
    - debug 有调试信息
    - release有优化
    - 如果要调整Debug和release 、平台的各种配置的话，需要每个进行他单独配置，比较麻烦，可以采用cmake来进行配置
- 工程操作：新建、添加、删除
- 重要工程属性修改

## lib静态库工程

- 简单Lib应用创建
- 通用依赖建立
- vs简化依赖建立

## dll动态库工程

- import导入dll工程建立
- 动态加载dll工程建立
- 简单插件工程

## Linux跨平台工程

# 工程组织篇

- 手动多工程组织
    - 编译排错
    - 工程选项修改
- cmake工程组织
- 版本管理

# 代码调试手段

- 常用调试手段
- 断电
- 多线程调试
- 性能内存监视
- 远程调试

  

# 参考链接

下载VC

- Tanks-1992
    - [github.com/krystiankaluzny/Tanks](http://github.com/krystiankaluzny/Tanks)
- SDL
    - [www.libsdl.org/download-2.0.php](http://www.libsdl.org/download-2.0.php)
- SDL TTF
    - [www.libsdl.org/porojects/SDL_ttf](http://www.libsdl.org/porojects/SDL_ttf)
- SDL Image
    - [www.libsdl.org/porojects/SDL_image](http://www.libsdl.org/porojects/SDL_image)
