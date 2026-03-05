---
tags:
  - C++
up:
  - "[[关键字、修饰符、操作符、宏等]]"
down:
relation:
  - "[[sizeof]]"
---
> sizeof是在编译时确定

---

- sizeof是一个操作符，strlen是库函数。
- sizeof的参数可以是数据的类型，也可以是变量，而strlen只能以结尾为‘\0’的字符串作参数。
- 编译器在编译时就计算出了sizeof的结果，而strlen函数必须在运行时才能计算出来。并且sizeof计算的是数据类型占内存的大小，而strlen计算的是字符串实际的长度。
- 数组做sizeof的参数不退化，传递给strlen就退化为指针了
