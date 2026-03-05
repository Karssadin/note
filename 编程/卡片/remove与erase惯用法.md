---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
---

`std::remove` / `std::remove_if` **不会真正删除元素**，只是将不需要的元素移到序列末尾，返回"新的逻辑末尾"迭代器。需要配合容器的 `erase` 方法才能真正删除。

## remove-erase 惯用法

```cpp
vector<int> v = {1, 2, 3, 2, 4, 2, 5};

// 错误理解：以为 remove 会删除元素
auto it = remove(v.begin(), v.end(), 2);
// 此时 v 的 size 未变！只是把 2 移到了末尾

// 正确用法：配合 erase
v.erase(remove(v.begin(), v.end(), 2), v.end());
// v = {1, 3, 4, 5}
```

## remove_if

按条件移除：

```cpp
v.erase(
    remove_if(v.begin(), v.end(), [](int x){ return x % 2 == 0; }),
    v.end()
);
```

## C++20 std::erase / std::erase_if

C++20 简化了这个惯用法：

```cpp
std::erase(v, 2);                                    // 删除所有 2
std::erase_if(v, [](int x){ return x % 2 == 0; });   // 删除所有偶数
```

## unique

移除**相邻**的重复元素（要求先排序），同样返回新逻辑末尾：

```cpp
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());
```
