---
tags: 
up: 
  - "[[常用函数、命名空间]]"
down: 
relation:
---
```C
\#include <ctime>
srand((unsigned int)time(NULL));
int num=rand();//可以用取余来锁定范围
```