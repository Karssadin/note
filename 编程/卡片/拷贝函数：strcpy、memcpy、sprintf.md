---
tags:
  - C++
up:
  - "[[C++]]"
down:
relation:
  - "[[字符串操作函数：strcpy、strlen、strcat、strcmp]]"
---
- strcpy、sprintf 与memcpy 都可以实现拷贝的功能，但是针对的对象不同。下面按照执行效率排序
- memcpy 的两个对象就是两个任意可操作的内存地址，并不限于何种数据类型。内存块间的拷贝
- strcpy 的两个操作对象均为字符串，实现字符串变量间的拷贝
- sprintf 的操作源对象可以是多种数据类型， 目的操作对象是字符串，实现其他数据类型格式到字符串的转化
