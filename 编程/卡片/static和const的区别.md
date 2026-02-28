---
tags: 
up: 
  - "[[关键字、修饰符、操作符、宏等]]"
down: 
relation:
  - "[[const和define的区别]]"
  - "[[constexpr和const的区别]]"
  - "[[常量：define、const]]"
  - "[[类型限定符：const、volatile]]"
  - "[[静态变量：static]]"
---
- 修饰常量
    - static：函数执行后不会释放其内存空间
    - const：超出作用域之后空间那就会被释放，在定义时必须初始化之后无法更改，const形参可以接受const和非const的实参
- 修饰成员变量
    - static：只能用在类定义体内部生命，外部初始化，且初始化不加static
    - const：在某个对象的生命周期内是常量，对整个对象而言是可变的。不能赋值，不能在类外定义，只能通过构造函数的参数初始化列表初始化，因为不同对象的const成员不同
- 修饰成员函数
    - static：作为类作用域的全局函数，只能访问静态成员和调用静态函数。不能声明为virtual。没有this指针，不能直接存取非类的静态成员，调用非静态成员函数
    - const: 防止成员函数修改对象的内容，const对象不可以调用非const的函数，非const对象可以调用const函数
- const和static不能同时修饰成员函数，静态函数不能含有this指针，不能实例化，而const成员函数必须具体到某一实例