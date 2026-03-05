---
tags:
  - C++
up:
  - "[[面向对象]]"
down:
relation:
  - "[[类型限定符：const、volatile]]"
---
- 常函数：成员函数后加const后我们称为这个函数为常函数
    - this指针的指向不可被修改：类 const this，如果在const成员函数中即为：const 类 const this
    - 常函数中this指针的指向被视为指向常对象的指针，常对象的成员变量在常函数中被视为const。不能通过该指针修改常对象的成员变量
    - 常函数内不可以修改成员属性，除非被声明为`mutalbe`
- 常对象：const修饰的对象称为常量对象
    - 其成员变量的值不能被修改
    - 常对象只能调用常函数，可以确保常对象的成员变量不会被修改

  

```C++
class MyClass {
public:
    mutable int x; //mutable成员变量，在常函数中可以修改
    const int y;
      // 构造函数中初始化 const 成员变量
    MyClass(int value) : y(value) {}
    void normalFunc() {} // 非常量成员函数
    void constFunc() const {} // 常量成员函数
};

int main() {
    const MyClass obj = {10};
    // obj.x = 20; // 错误，常量对象的成员变量不可修改
    return 0;
}
```
