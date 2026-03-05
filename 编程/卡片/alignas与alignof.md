---
tags:
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[结构体内存对齐]]"
---

C++11 引入 `alignas` 和 `alignof`，提供标准化的内存对齐控制能力。

## alignof：查询对齐要求

```cpp
alignof(char);      // 1
alignof(int);       // 4（通常）
alignof(double);    // 8（通常）
alignof(std::max_align_t);  // 平台最大自然对齐

struct S { int a; double b; };
alignof(S);         // 8（按最大成员对齐）
```

## alignas：指定对齐方式

```cpp
// 要求 16 字节对齐
struct alignas(16) CacheLine {
    int data[4];
};

// 变量对齐
alignas(64) int buffer[256];  // 64 字节对齐，适配缓存行

// 不能比自然对齐更小
// alignas(1) double d;  // 错误或未定义行为
```

## 应用场景

1. **SIMD 优化**：SSE/AVX 要求 16/32 字节对齐
2. **缓存行对齐**：避免 false sharing，`alignas(64)` 对齐到 CPU 缓存行
3. **硬件 DMA**：某些硬件要求特定对齐的缓冲区
4. **自定义内存池**：保证分配的内存满足对齐要求

```cpp
// 避免 false sharing
struct alignas(64) Counter {
    std::atomic<int> value{0};
};

Counter counters[4];  // 每个 Counter 独占一条缓存行
```

## C++17 增强

C++17 保证 `new` 表达式能正确处理过对齐类型（over-aligned types），不再需要自定义 `operator new`。
