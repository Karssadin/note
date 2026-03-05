---
tags:
  - C++
up:
down:
relation:
  - "[[可变参数模板]]"
  - "[[可变参数宏——Variadic Macros]]"
---
```C
#include <cstdarg>
#include <iostream>

void printInts(int count, ...) {
    va_list args;
    va_start(args, count);    // 初始化 args，指定最后一个固定参数是 count
    for (int i = 0; i < count; ++i) {
        int value = va_arg(args, int);  // 取出下一个参数（类型需手动指定）
        std::cout << value << " ";
    }
    va_end(args);             // 清理
    std::cout << "\n";
}

int main() {
    printInts(3, 10, 20, 30);  // count = 3
    printInts(5, 1, 2, 3, 4, 5);
}
```
- **必须手动传入参数数量**（或靠 sentinel，如 `NULL`），否则不知道多少个参数。
- **必须自己知道参数类型**（`va_arg` 要写具体类型），没有类型检查。
- **易出错**：比如传 double 用 int 取，就 UB（未定义行为）。
