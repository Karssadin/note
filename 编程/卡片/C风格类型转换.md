---
tags:
  - C++
up:
  - "[[类型转换]]"
down:
relation:
---

C 风格类型转换使用 `(T)expr` 或 `T(expr)` 语法，功能强大但不安全，C++ 中应使用四种命名转换替代。

## 语法

```cpp
double x = 3.14;
int y = (int)x;       // C 风格
int z = int(x);       // 函数风格（等价）
```

## 危险性

C 风格转换会依次尝试以下转换，选第一个能成功的，程序员无法控制：

1. `const_cast`（去 const）
2. `static_cast`（隐式可逆转换）
3. `static_cast` + `const_cast`
4. `reinterpret_cast`（二进制重解释）
5. `reinterpret_cast` + `const_cast`

```cpp
const int* p = &val;
int* q = (int*)p;          // 静默去 const，可能导致未定义行为
void* v = (void*)some_ptr;  // 静默 reinterpret，编译器不警告
```

## C++ 命名转换替代

| 转换 | 用途 |
|------|------|
| [[static_cast]] | 基本类型转换、向上/向下转型 |
| [[dynamic_cast]] | 运行期多态转型，带检查 |
| [[const_cast]] | 添加/移除 const |
| [[reinterpret_cast]] | 指针的二进制重解释 |

命名转换的优势：意图明确、便于搜索、编译器可更严格检查。
