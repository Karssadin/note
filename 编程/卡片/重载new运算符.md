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
```C
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