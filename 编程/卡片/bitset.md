---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
---
- 压位
- bool数组，压入字节内部，而不是单个字节存储bool，占用空间小了8倍
- bitset<10000> b;
- `[]`取出来 某一位
- `count`返回有多少个1
- `any/none`：返回是否至少有一个1、判断是否全为0
- `set()`所有位置为1
- `set(k,v)`，将k位变成v
- `reset()`所有位置位0
- `flip()`所有取反
- `flip(k)`第k位取反