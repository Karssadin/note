---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
  - "[[priority_queue]]"
---

- 先进先出，有两个出口
- 队头只能pop，队尾只能push
- 队列不允许有遍历行行为

### 常用接口

- 构造函数:
    - `queue<T> que;` `queue`采用模板类实现 `queue`对象的默认构造形式
    - `queue(const queue &que);` 拷贝构造函数
- 赋值操作:
    - `queue& operator=(const queue &que);`重载等号操作符
- 数据存取:
    - `push(elem);` 往队尾添加元素
    - `pop();` 从队头移除第一个元素
    - `back();` 返回最后一个添加的元素
    - `front();` 返回第一个元素
- 大小操作:
    - `empty();` 判断堆栈是否为空
    - `size();` 返回栈的大小
- 没有`clear`函数