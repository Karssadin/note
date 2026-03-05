---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
---
头文件 `<algorithm>`，原地反转序列 `[beg, end)` 中的元素顺序。

## 用法

```cpp
vector<int> v = {1, 2, 3, 4, 5};
reverse(v.begin(), v.end());       // v = {5, 4, 3, 2, 1}
reverse(v.begin(), v.begin() + 3); // 反转前 3 个：v = {3, 4, 5, 2, 1}

string s = "hello";
reverse(s.begin(), s.end());       // s = "olleh"
```

- `beg`：开始迭代器
- `end`：最后一个元素之后的位置
- 要求双向迭代器（list、vector、deque、string 均可）

## reverse_copy

反转后输出到另一个容器，不修改原序列：

```cpp
vector<int> src = {1, 2, 3};
vector<int> dst(3);
reverse_copy(src.begin(), src.end(), dst.begin());
// dst = {3, 2, 1}，src 不变
```
