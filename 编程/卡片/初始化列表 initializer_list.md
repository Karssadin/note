---
tags:
  - STL
up:
  - "[[变量和常量]]"
down:
relation:
  - "[[列表初始化]]"
  - "[[构造函数的初始化列表]]"
---
- 它是一个 **轻量级的只读数组封装**，提供 **元素数量和迭代器访问**。
- 本质上就是一个对象，包含 **指向数组首元素的指针和大小**。
- **不是动态数组**，长度固定，元素不可修改。

1. 类成员初始化
```C++
#include <vector>

class MyVector {
    std::vector<int> data;
public:
    MyVector(std::initializer_list<int> il) : data(il) {}
};

int main() {
    MyVector v{1,2,3};  // 调用 initializer_list 构造函数
}

```
2. 局部变量使用
```C++
#include <initializer_list>
#include <iostream>

int main() {
    std::initializer_list<int> il = {1,2,3,4};
    for (int x : il) std::cout << x << " ";
}
```
3. 普通函数中使用
```C++
#include <initializer_list>
#include <iostream>

void printAll(std::initializer_list<int> il) {
    for (int x : il) {
        std::cout << x << " ";
    }
    std::cout << "\n";
}

int main() {
    printAll({1, 2, 3, 4, 5});  // ✅ 可以直接用列表初始化调用
}
```
4. 初始化容器
```C++
#include <vector>
#include <algorithm>
#include <initializer_list>
#include <iostream>

int main() {
    std::initializer_list<int> il = {3,1,4,2};
    std::vector<int> v(il);   // 可以直接初始化容器
    std::sort(v.begin(), v.end());
    for (int x : v) std::cout << x << " ";
}
```
