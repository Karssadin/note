---
tags:
  - C11
up:
  - "[[可变参数模板]]"
down:
relation:
  - "[[列表初始化]]"
  - "[[初始化列表 initializer_list]]"
---
**Expander hack** 是 C++11 里出现的一种编程技巧，用来 **展开可变参数模板参数包** 并保证展开时的副作用按照参数顺序执行。

- 利用 **列表初始化或初始化列表保证严格左到右的求值顺序**，
- 再配合逗号表达式 `(expr, 0)` 把每个副作用表达式变成合法的整型元素，
- 从而触发副作用（比如打印），并依次展开整个参数包。


- 有如下几种用法：
```C++
using expander = int[];
(void)expander{0, ( (std::cout << args), 0 )...};

int dummy[] = {0, ( (std::cout << args), 0 )...};
(void)dummy;

(void)std::initializer_list<int>{ (std::cout << args << " ", 0)... };
```
- 开头添加(void)是**防止编译器报“未使用的临时对象/变量”警告**。
---
- 示例：
```C++
template<typename... Args>
void print(Args... args) {
    (void)std::initializer_list<int>{ (std::cout << args << " ", 0)... };
    std::cout << '\n';
}
```
```C++
#include <iostream>
#include <utility>

template<typename... Args>
void print2(Args&&... args) {
    using expander = int[];
    (void)expander{
        0,  // dummy value 保证非空
        ( (std::cout << std::forward<Args>(args) << " "), 0 )...//这里也可以是其他函数，调用其他函数
    };
    std::cout << '\n';
}

int main() {
    print2(1, 3.14, "hello", 'c');
}
```

```C++
#include <iostream>

template<typename... Args>
void add_all(int& sum, Args... args) {
    using expander = int[];
    (void)expander{
        0,
        ( (sum += args), 0 )...
    };
}

int main() {
    int total = 0;
    add_all(total, 1, 2, 3, 4, 5);
    std::cout << "sum = " << total << "\n";
}
```
