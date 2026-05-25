---
tags:
  - C++
  - STL
up:
  - "[[容器、适配器、工具]]"
down:
relation:
---
- [[#常用函数、迭代器|常用函数、迭代器]]
- [[#构造函数|构造函数]]
- [[#赋值操作|赋值操作]]
- [[#大小操作|大小操作]]
- [[#插入和删除|插入和删除]]
- [[#数据存取|数据存取]]
----

## 底层原理

- 底层：分段连续空间 + 中控器（map 指针数组，每个指针指向一个固定大小的缓冲区）
- 头尾插入删除 O(1)，随机访问 O(1)（但比 vector 慢，需要计算段+偏移）
- 没有 capacity 概念，不需要 reserve，空间可双向扩展
- 迭代器复杂（需维护当前缓冲区、缓冲区边界、中控器指针），排序时建议先复制到 vector 排序再复制回来
- 释放内存时会释放空缓冲区，不像 vector 只增不减

与 vector 的区别：
- vector 头部插入删除效率低，deque 头部操作 O(1)
- vector 连续内存、缓存友好，deque 分段连续、缓存相对不友好
    - vector访问元素时的速度会比deque快,这和两者内部实现有关
- deque内部有个中控器，维护每段缓冲区中的内容，缓冲区中存放真实数据
- 中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间
- deque的迭代器也是支持随机访问的

### 常用函数、迭代器

- `push_front` `pop_front` `push_back` `pop_back`
- `front` `back` `begin` `end`

### 构造函数

- `deque<T> deqT;` 默认构造形式
- `deque(beg, end);` 构造函数将[beg, end)区间中的元素拷贝给本身。
- `deque(n, elem);` 构造函数将n个elem拷贝给本身
- `deque(const deque &deq);`拷贝构造函数

### 赋值操作

- `deque& operator=(const deque &deq);` 重载等号操作符
- `assign(beg, end) ;` 将[beg, end)区间中的数据拷贝赋值给本身。
- `assign(n,elem);` 将n个elem拷贝赋值给本身。

### 大小操作

没有容量操作，只有个数

- `deque.empty();`判断容器是否为空
- `deque.size();` 返回容器中元素的个数
- `deque.resize(num);`
    - 重新指定容器的长度为num,若容器变长，则以默认值填充新位置
    - 如果容器变短，则末尾超出容器长度的元素被删除。
- `deque.resize(num,elem);`
    - 重新指定容器的长度为num,若容器变长，则以elem值填充新位置
    - 如果容器变短，则末尾超出容器长度的元素被删除

### 插入和删除

- 两端操作
    - `push_back(elem);`在容器尾部添加一个数据
    - `push_front(elem);` 在容器头部插入一个数据
    - `pop_back();` 删除容器最后一个数据
    - `pop_front();` 删除容器第一个数据
- 指定位置操作:
    - `insert(pos,elem);`在pos位置插入一个elem元素的拷贝，返回新数据的位置。
    - `insert(pos,n,elem);` 在pos位置插入n个elem数据，无返回值。
    - `insert(pos,beg,end);` 在pos位置插入[beg,end)区间的数据，无返回值。
    - `clear();` 清空容器的所有数据
    - `erase(beg,end);` 删除[beg,end)区间的数据，返回下一个数据的位置。
    - `erase(pos);` 删除pos位置的数据，返回下一个数据的位置。

### 数据存取

- `at(int idx);` 返回索引idx所指的数据
- `operator[];` 返回索引|idx所指的数据
- `front();` 返回容器中第一个数据元素
- `back();` 返回容器中最后一个数据元素
