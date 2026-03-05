---
tags:
up:
  - "[[常用函数、命名空间]]"
down:
relation:
---

`rand()` 生成伪随机整数，范围 `[0, RAND_MAX]`。需要先用 `srand()` 设置种子，否则每次运行结果相同。

```cpp
#include <cstdlib>
#include <ctime>

srand((unsigned int)time(NULL));  // 用当前时间作为种子
int num = rand();                 // [0, RAND_MAX]
int dice = rand() % 6 + 1;       // [1, 6]
int range = rand() % (b - a + 1) + a;  // [a, b]
```

C++11 推荐使用 `<random>` 替代，分布更均匀、线程安全：

```cpp
#include <random>
std::mt19937 gen(std::random_device{}());
std::uniform_int_distribution<int> dist(1, 100);
int val = dist(gen);  // [1, 100] 均匀分布
```

`rand() % n` 在 n 不是 RAND_MAX+1 的因数时分布不均匀，`<random>` 无此问题。
