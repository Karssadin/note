---
tags:
  - 操作系统
up:
  - "[[C++新特性]]"
down:
relation:
---

C++17 引入 `std::optional<T>`，安全地表示"可能有值，也可能无值"的语义。

## 替代方案对比

| 旧方案 | 问题 |
|--------|------|
| 返回指针 + nullptr | 所有权语义不清 |
| 哨兵值（-1, INT_MAX） | 占用合法值空间 |
| 输出参数 + bool 返回值 | 接口不自然 |

## 基本用法

```cpp
#include <optional>

std::optional<int> find_index(const std::vector<int>& v, int target) {
    for (int i = 0; i < v.size(); i++)
        if (v[i] == target) return i;
    return std::nullopt;  // 无值
}

auto result = find_index(vec, 42);
if (result.has_value()) {
    std::cout << "found at " << result.value();
    std::cout << "found at " << *result;  // 等价
}

// value_or 提供默认值
int idx = result.value_or(-1);
```

## 注意事项

- `optional<T>` 内部存储 T 的对齐副本 + bool 标志，不使用堆分配
- 不要用 `optional<T&>`（不支持），用 `optional<reference_wrapper<T>>`
- 访问空 optional 的 `.value()` 会抛 `std::bad_optional_access`
