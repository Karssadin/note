---
tags:
up:
  - "[[内存管理]]"
down:
relation:
  - "[[new-delete、malloc-free的区别]]"
  - "[[方法的重写、重载、隐藏]]"
  - "[[运算符重载]]"
---

通过重载 `operator new` / `operator delete`，可以自定义对象的内存分配策略，常用于内存池、调试追踪、对齐控制等场景。

- 重载为类的**静态成员函数**（即使不写 `static`，编译器也会隐式处理）
- 全局重载影响所有 `new` 表达式，谨慎使用

```cpp
class MyClass {
public:
    // 重载 operator new
    static void* operator new(std::size_t size) {
        std::cout << "Custom operator new for MyClass\n";
        return ::operator new(size);  // 使用全局 operator new
    }
    // 重载 operator delete
    static void operator delete(void* memory) {
        std::cout << "Custom operator delete for MyClass\n";
        ::operator delete(memory);  // 使用全局 operator delete
    }
};
```
