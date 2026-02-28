---
tags:
up:
  - "[[C++]]"
  - "[[C++新特性]]"
down:
relation:
  - "[[类型转换]]"
---
  

- RTTI即运行时类型识别`Runtime Type Identification`，其功能由：
    - typeid运算符，用于返回表达式的类型，可以通过基类的指针获取派生类的数据类型；
    - dynamic_cast运算符，具有类型检查的功能，用于将基类的指针或引用安全地转换成派生类的指针或引用。
    - type_info结构存储了有关特定类型的信息。