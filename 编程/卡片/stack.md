---
tags:
  - C++
  - STL
up:
  - "[[容器、适配器、工具]]"
down:
relation:
---

`std::stack` 是后进先出（LIFO）的容器适配器，只能访问栈顶元素，不允许遍历。

## 底层实现

- 默认底层容器为 `deque`，也可指定为 `vector` 或 `list`：
  ```cpp
  stack<int> s1;                        // 底层 deque
  stack<int, vector<int>> s2;           // 底层 vector
  stack<int, list<int>> s3;             // 底层 list
  ```
- 不能遍历的原因：stack 刻意限制接口，只暴露 `top()`，封装了底层容器的 `begin()`/`end()`

## 常用接口

| 操作 | 说明 |
|------|------|
| `push(elem)` | 向栈顶添加元素 |
| `pop()` | 移除栈顶元素（不返回） |
| `top()` | 返回栈顶元素（不移除） |
| `empty()` | 判断是否为空 |
| `size()` | 返回栈的大小 |

- 构造：`stack<T> stk;`（默认构造）、`stack(const stack& stk);`（拷贝构造）
- 赋值：`stack& operator=(const stack& stk);`
- 没有 `clear()` 函数，清空需循环 `pop()` 或重新赋值

## 应用场景

- 括号匹配：遇到左括号入栈，遇到右括号弹栈比较
- 表达式求值：中缀转后缀（逆波兰）、后缀表达式计算
- 函数调用栈模拟
- DFS 非递归实现
