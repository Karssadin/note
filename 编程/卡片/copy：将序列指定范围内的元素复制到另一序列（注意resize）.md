---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
---
头文件 `<algorithm>`，将序列 `[beg, end)` 中的元素复制到目标位置。

## 基本用法

```cpp
vector<int> src = {1, 2, 3, 4, 5};
vector<int> dst(5);  // 必须预分配空间！
copy(src.begin(), src.end(), dst.begin());
```

- `beg`：源序列开始迭代器
- `end`：源序列结束迭代器
- `dest`：目标起始迭代器

**注意：目标容器必须有足够空间**，否则行为未定义。先 `resize()` 或 `reserve()` + `back_inserter`。

## 常见变体

```cpp
// copy_if：按条件拷贝
vector<int> evens;
copy_if(src.begin(), src.end(), back_inserter(evens),
    [](int x) { return x % 2 == 0; });

// copy_n：拷贝前 n 个
copy_n(src.begin(), 3, dst.begin());

// copy_backward：从后向前拷贝（用于重叠区间向后移动）
copy_backward(src.begin(), src.begin() + 3, dst.end());
```

## back_inserter 技巧

当目标容器大小不确定时，使用 `back_inserter` 自动 `push_back`：

```cpp
vector<int> dst;  // 无需预分配
copy(src.begin(), src.end(), back_inserter(dst));
```
