---
tags:
  - C++
up:
  - "[[C++基础知识]]"
down:
relation:
  - "[[程序编译流程]]"
  - "[[GCC、G++]]"
---

C++ 标准由 ISO 委员会制定（如 C++11、C++14、C++17、C++20），定义了语言的语法规则和标准库接口。但不同编译器对标准的实现存在差异：

## 主要编译器

| 编译器 | 平台 | 特点 |
|--------|------|------|
| GCC (g++) | Linux / 跨平台 | 开源，标准支持最全面 |
| Clang (clang++) | macOS / 跨平台 | 错误提示友好，编译速度快 |
| MSVC (cl.exe) | Windows | Visual Studio 内置，对 Windows API 支持最好 |

## 常见差异

1. **扩展语法**：GCC 支持 `__attribute__`、变长数组（VLA）等非标准扩展；MSVC 使用 `__declspec`
2. **模板实例化**：不同编译器对 two-phase lookup 的实现程度不同，可能导致代码在一个编译器通过而另一个报错
3. **ABI 兼容性**：不同编译器（甚至同一编译器的不同版本）的名称修饰（name mangling）规则不同，二进制库不能混用
4. **标准库实现**：libstdc++（GCC）、libc++（Clang）、MSVC STL 三者的容器内存布局和性能特性有差异
5. **预定义宏**：`__GNUC__`、`__clang__`、`_MSC_VER` 用于识别编译器

## 实践建议

- 使用 `-std=c++17`（GCC/Clang）或 `/std:c++17`（MSVC）指定标准版本
- 使用 `-Wall -Wextra -pedantic` 开启严格警告，减少对编译器扩展的依赖
- 跨平台项目使用 CMake 管理构建，统一编译选项
