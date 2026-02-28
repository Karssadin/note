---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
---
### pair的创建

- `pair<type,type> p{value1, value2};`
- `pair<type,type> p = make_pair( value1，value2 );`利用first 和second选择元素
- type也可以是pair
- pair支持比较运算，以first为第一个关键字，以second为第二个关键字。