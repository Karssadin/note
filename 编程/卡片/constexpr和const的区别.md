---
tags: 
up: 
  - "[[关键字、修饰符、操作符、宏等]]"
down: 
relation:
  - "[[static和const的区别]]"
  - "[[常量：define、const]]"
  - "[[const和define的区别]]"
---
- const标识只读，constexpr标识常量
- 将一个成员函数标记为constexpr，则顺带也将它标记为了const。如果你将一个变量标记为constexpr，则同样它是const的。但相反并不成立，一个const的变量或函数，并不是constexpr的

---

> constexpr变量

- 将变量声明为constexpr类型，由编译器来验证变量的值是否是一个常量
- constexpr指针的方向不能改变，指向的对象可以改变，与int* const一致

> constexpr函数

- 指能用于常量表达式的函数
- 返回类型和所有形参类型都是字面值类型，函数体有且只有一条return语句：`constexpr int new() {return 42;}`
- constexpr函数被隐式转换成了内联函数, constexpr和内联函数可以在程序中多次定义，一般定义在头文件

> constexpr构造函数

- 构造函数必须有一个空的函数体，即所有成员变量的初始化都放到初始化列表中。
- 对象调用的成员函数必须使用constexpr修饰

---

- constexpr的好处
    - 为一些不能修改数据提供保障，写成变量则就有被意外修改的风险
    - 有些场景，编译器可以在编译期对constexpr的代码进行优化，提高效率
    - 相比宏来说，没有额外的开销，但更安全可靠