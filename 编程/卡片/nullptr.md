---
tags:
  - C++
up:
  - "[[C++]]"
  - "[[指针]]"
down:
relation:
  - "[[指针]]"
---
- nullptr是用来代替NULL，一般C++会把NULL、0视为同一种东西，这取决去编译器如何定义NULL，有的定义为((void*)0)，有的定义为0
- C不允许直接将void* 隐式转换到其他类型，在进行C重载时会发生混乱
    - 如果NULL被定义为 ((void_)0)，那么当编译char_ ch = NULL时，NULL被定义为 0
    - 当foo(NULL)时，此时NULL为0，会去调用foo(int )，从而发生混乱
- 为解决这个问题，从而需要使用NULL时，用nullptr代替:C++11引入nullptr关键字来区分空指针和0。nullptr 的类型为nullptr_t，能够转换为任何指针或成员指针的类型，也可以进行相等或不等的比较。
