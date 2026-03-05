---
tags:
up:
  - "[[STL]]"
down:
  - "[[STL优点]]"
  - "[[迭代器失效]]"
relation:
---

STL（Standard Template Library）是 C++ 标准库的核心部分，基于泛型编程思想，提供通用的数据结构和算法。

## 六大组件

| 组件 | 说明 | 示例 |
|------|------|------|
| 容器（Container） | 管理数据的存储 | vector、map、set |
| 算法（Algorithm） | 操作容器中的数据 | sort、find、count |
| 迭代器（Iterator） | 容器与算法之间的桥梁 | begin()、end() |
| 仿函数（Functor） | 重载 `operator()` 的类对象 | greater、less |
| 适配器（Adapter） | 修饰容器/仿函数/迭代器的接口 | stack、queue |
| 空间配置器（Allocator） | 管理内存分配与释放 | std::allocator |

## 容器分类

```
容器
├── 序列容器：vector、list、deque、string、array、forward_list
├── 关联容器：set、multiset、map、multimap（红黑树）
├── 无序容器：unordered_set、unordered_map（哈希表）
└── 适配器：stack、queue、priority_queue
```

## 迭代器分类

| 类型 | 能力 | 典型容器 |
|------|------|---------|
| 输入/输出迭代器 | 单向只读/只写 | istream/ostream |
| 前向迭代器 | 单向读写 | forward_list |
| 双向迭代器 | 双向读写 | list、set、map |
| 随机访问迭代器 | 随机跳转 | vector、deque |

## 设计思想

- **泛型编程**：用模板将数据类型参数化，一套代码适用于所有类型
- **解耦**：容器不需要知道算法的存在，算法不需要知道容器的内部结构，迭代器作为统一接口连接二者
- **复杂度承诺**：每个容器和算法都有明确的时间复杂度保证，便于性能分析

相关：[[STL优点]]、[[迭代器失效]]
