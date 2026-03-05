---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
---

## swap 算法

`std::swap` 交换两个对象的值；容器的 `swap` 成员函数在 O(1) 内交换两个容器的所有元素（仅交换内部指针，不移动元素）。

### 头文件

```cpp
#include <algorithm>  // std::swap
```

### 语法

```cpp
swap(T& a, T& b);                          // 交换两个同类型对象
swap(container c1, container c2);          // 交换两个同类型容器（等价于 c1.swap(c2)）
```

### 示例

```cpp
// 交换两个变量
int a = 1, b = 2;
std::swap(a, b);  // a=2, b=1

// 交换两个 vector
std::vector<int> v1 = {1, 2, 3};
std::vector<int> v2 = {4, 5, 6, 7};
std::swap(v1, v2);
// v1 = {4,5,6,7}, v2 = {1,2,3}，O(1) 复杂度

// 容器成员函数版
v1.swap(v2);  // 等价

// 利用 swap 释放内存（shrink to fit 技巧）
std::vector<int> v;
v.push_back(1);
std::vector<int>().swap(v);  // v 的内存被释放
```

### iter_swap：交换迭代器指向的元素

```cpp
#include <algorithm>
std::vector<int> v = {1, 2, 3, 4};
std::iter_swap(v.begin(), v.begin() + 3);  // 交换第1和第4个元素
// v = {4, 2, 3, 1}
```

### 复杂度

| 操作 | 复杂度 |
|------|--------|
| 内置类型/小对象 swap | O(1) |
| 容器 swap（vector/map 等） | O(1)（交换内部指针） |
| 数组 swap | O(n) |

### 自定义类型 swap 最佳实践

```cpp
class MyClass {
    std::vector<int> data;
public:
    // 提供成员 swap，避免复制
    void swap(MyClass& other) noexcept {
        data.swap(other.data);
    }
    // 友元 swap，支持 ADL
    friend void swap(MyClass& a, MyClass& b) noexcept {
        a.swap(b);
    }
};
```
