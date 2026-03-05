---
tags:
up:
  - "[[容器、适配器、工具]]"
down:
relation:
  - "[[queue]]"
---

`std::priority_queue` 是优先级队列，每次取出的元素是当前队列中优先级最高的。

## 底层实现

- 底层容器默认为 `vector`，通过堆算法（`make_heap`、`push_heap`、`pop_heap`）维护
- 默认大根堆（`std::less<T>`），堆顶是最大元素
- 不提供迭代器和遍历机制（只能访问堆顶）

## 大根堆与小根堆

```cpp
// 大根堆（默认）
priority_queue<int> maxHeap;

// 小根堆
priority_queue<int, vector<int>, greater<int>> minHeap;

// 技巧：插入相反数实现小根堆
priority_queue<int> pq;
pq.push(-x);  // 取出时取反
```

## 自定义比较器

```cpp
// 仿函数
struct Cmp {
    bool operator()(int a, int b) { return a > b; }  // 小根堆
};
priority_queue<int, vector<int>, Cmp> pq1;

// lambda（需要 decltype）
auto cmp = [](int a, int b) { return a > b; };
priority_queue<int, vector<int>, decltype(cmp)> pq2(cmp);
```

## 常用操作

| 操作 | 说明 | 复杂度 |
|------|------|--------|
| `push(x)` | 插入元素 | O(log n) |
| `pop()` | 弹出堆顶 | O(log n) |
| `top()` | 返回堆顶（不弹出） | O(1) |
| `size()` | 元素个数 | O(1) |
| `empty()` | 是否为空 | O(1) |

- 没有 `clear()` 方法，清空需要循环 `pop()` 或重新赋值

## 应用场景

- Top-K 问题：维护大小为 K 的小根堆，遍历结束后堆中即为最大的 K 个元素
- 合并 K 个有序链表：每次取各链表头的最小值
- Dijkstra 最短路算法
