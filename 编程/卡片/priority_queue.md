---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
  - "[[queue]]"
---
- 优先队列堆，默认是大根堆
- 小根堆：
    - 插入，相反数
    - 定义成为小根堆`priority_queue<int> heap;priority_queue<int,vector<int>,greater> heap;`
- `push()`插入一个元素
- `top()` 返回堆顶元素
- `pop()` 弹出堆顶元素
- `size()`
- `empty()`
- 没有`clear()`