---
tags:
up:
  - "[[常用函数、命名空间]]"
down:
relation:
---

`std::numeric_limits<T>` 提供各数值类型的编译期属性查询，位于 `<limits>` 头文件中，是类型安全的替代 C 语言 `INT_MAX` / `DBL_MIN` 等宏的方式。

常用成员：`min()`（最小正值/最小值）、`max()`（最大值）、`lowest()`（最小值，含负）、`epsilon()`（浮点精度）、`infinity()`、`is_signed`。

```cpp
\#include <iostream>
\#include <limits>
using namespace std;

int main() {

    std::cout << "Int Min " << std::numeric_limits<int>::min() << endl;
    std::cout << "Int Max " << std::numeric_limits<int>::max() << endl;
    std::cout << "Unsigned Int  Min " << std::numeric_limits<unsigned int>::min() << endl;
    std::cout << "Unsigned Int Max " << std::numeric_limits<unsigned int>::max() << endl;
    std::cout << "Long Int Min " << std::numeric_limits<long int>::min() << endl;
    std::cout << "Long Int Max " << std::numeric_limits<long int>::max() << endl;

    std::cout << "Unsigned Long Int Min " << std::numeric_limits<unsigned  long int>::min() <<endl;
    std::cout << "Unsigned Long Int Max " << std::numeric_limits<unsigned  long int>::max() << endl;
```
