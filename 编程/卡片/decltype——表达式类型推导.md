---
tags:
  - C11
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[auto——自动类型推导]]"
  - "[[auto与decltype结合使用——返回类型后置语法的应用]]"
---
- decltype则用于推导表达式类型，这里只用于编译器分析表达式的类型，表达式实际不会进行运算
- decltype不会像auto一样忽略引用和cv属性，decltype会保留表达式的引用和cv属性，对于decltype(exp)
    - exp是表达式，decltype(exp)和exp类型相同,返回变量的类型
    - exp是函数调用，decltype(exp)和函数返回值类型相同，会返回对应的函数类型，不会自动转换成相应的函数指针。
    - 其它情况，exp的结果是左值得到类型的引用，结果是右值的exp得到类型；