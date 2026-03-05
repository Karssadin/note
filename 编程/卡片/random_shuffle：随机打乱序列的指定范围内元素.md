---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
---
- `random_shuffle(iterator beg, iterator end) ;`指定范围内的元素随机调整次序
    - `beg`：开始迭代器
    - `end`：结束迭代器

```C
\#include <ctime>
srand((unsigned int)time(NULL));
```
