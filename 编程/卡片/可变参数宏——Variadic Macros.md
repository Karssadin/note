---
tags:
  - C++
up:
down:
relation:
  - "[[传统C风格可变参数]]"
  - "[[可变参数模板]]"
---
- C99 引入了 **`...` 和 `__VA_ARGS__`**，用于写“可变参数宏”。
```C
#include <cstdio>

// 定义一个简单的日志宏

#define LOG(fmt, ...) \
    std::printf("[LOG] " fmt "\n", ##__VA_ARGS__)

int main() {
    LOG("hello world");
    LOG("value = %d, %f", 42, 3.14);
}

```

- `__VA_ARGS__` 会被替换为宏调用中的参数列表。
- `##__VA_ARGS__` 语法（GNU 扩展，后来标准化）允许在没有参数时安全移除前导逗号。
- 常用于日志、调试封装
