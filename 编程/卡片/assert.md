---
tags:
  - C++
up:
  - "[[关键字、修饰符、操作符、宏等]]"
down:
relation:
---
- 断言，是宏，而非函数。assert 宏的原型定义在 <assert.h>（C）、<cassert>（C++）中，其作用是如果它的条件返回错误，则终止程序执行。可以通过定义 NDEBUG 来关闭 assert，但是需要在源代码的开头，include <assert.h> 之前。
