---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
---
头文件 `<numeric>`，计算序列指定范围内元素的累积总和（或自定义二元操作的累积结果）。

## 基本用法

```cpp
#include <numeric>

vector<int> v = {1, 2, 3, 4, 5};
int sum = accumulate(v.begin(), v.end(), 0);      // 15
int sum10 = accumulate(v.begin(), v.end(), 10);    // 25（初始值 10）
```

- `beg`：开始迭代器
- `end`：结束迭代器
- `value`：累加起始值（**决定了返回值类型**，int 型初始值会导致浮点截断）

## 自定义操作

第四个参数可传入二元操作，替代默认的加法：

```cpp
// 求乘积
int product = accumulate(v.begin(), v.end(), 1, multiplies<int>());

// 字符串拼接
vector<string> words = {"hello", " ", "world"};
string result = accumulate(words.begin(), words.end(), string(""));

// lambda
int sum_sq = accumulate(v.begin(), v.end(), 0,
    [](int acc, int x) { return acc + x * x; });
```

## 注意

- 初始值类型决定返回值类型：`accumulate(v.begin(), v.end(), 0)` 用 int 累加，`0.0` 才用 double
- C++17 提供 `reduce`，支持并行执行，但要求操作满足交换律和结合律
