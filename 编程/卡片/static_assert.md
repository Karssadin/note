---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[constexpr函数]]"
  - "[[type_traits]]"
---

`static_assert` 是 C++11 引入的编译期断言，在编译阶段检查条件是否成立，不成立则直接报错，不产生任何运行期开销。

## 基本语法

```cpp
// C++11：必须提供错误消息
static_assert(sizeof(int) == 4, "int must be 4 bytes");

// C++17：错误消息可省略
static_assert(sizeof(int) == 4);
```

## 与 assert 对比

| 特性 | `static_assert` | `assert`（`<cassert>`） |
|------|----------------|------------------------|
| 检查时机 | 编译期 | 运行期 |
| 条件要求 | 常量表达式 | 任意表达式 |
| 失败行为 | 编译错误 | 终止程序（abort） |
| 运行期开销 | 无 | 有（Debug 模式） |

## 典型场景

```cpp
// 检查类型大小
static_assert(sizeof(void*) == 8, "requires 64-bit platform");

// 配合 type_traits
template <typename T>
T safe_add(T a, T b) {
    static_assert(std::is_arithmetic_v<T>, "T must be arithmetic type");
    return a + b;
}

// 检查结构体布局
struct Packet {
    uint32_t header;
    uint8_t  data[12];
};
static_assert(sizeof(Packet) == 16, "Packet layout mismatch");
```
