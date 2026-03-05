---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
---

C++17 引入 `std::any`，可以存储任意类型的单个值，是类型擦除容器。

## 基本用法

```cpp
#include <any>

std::any a = 42;
a = std::string("hello");
a = 3.14;

// 取值：std::any_cast
double d = std::any_cast<double>(a);       // OK
// int i = std::any_cast<int>(a);          // 抛 std::bad_any_cast

// 安全取值
if (auto* p = std::any_cast<double>(&a)) {
    std::cout << *p;
}

// 检查类型
a.type() == typeid(double);  // true
a.has_value();               // true
a.reset();                   // 清空
```

## 与 variant 对比

| 特性 | `std::any` | `std::variant` |
|------|-----------|----------------|
| 可存储类型 | 任意 | 预定义列表 |
| 类型检查 | 运行期 | 编译期 |
| 性能 | 可能堆分配 | 栈上，无堆分配 |
| 适用场景 | 配置系统、脚本绑定 | 已知的有限类型集 |

优先使用 `variant`；只在类型完全未知时使用 `any`。
