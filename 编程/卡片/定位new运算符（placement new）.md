---
tags: 
up: 
  - "[[内存管理]]"
down: 
relation:
  - "[[new-delete、malloc-free的区别]]"
---
- `new`运算符还有另一种变体：定位`new`运算符（`placement new`）
    - 允许在指定的内存位置上构造对象，常用于内存池或其他需要精确控制内存布局的场景。
    - 使用 `placement new` 后，需要手动调用析构函数来销毁对象，因为 `delete` 不能与 `placement new` 一起使用。

  

```C++
\#include <iostream>
\#include <new>  // for placement new

int main() {
    char buffer[sizeof(int)];  // 预分配一块内存
    int* p = new (buffer) int(42);  // 在预分配的内存中构造一个 int 对象

    std::cout << "Value: " << *p << std::endl;  // 输出值

    // 手动调用析构函数，因为我们使用了 placement new
    p->~int();

    return 0;
}
```