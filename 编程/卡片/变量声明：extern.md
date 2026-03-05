---
tags:
  - C++
up:
  - "[[变量和常量]]"
  - "[[关键字、修饰符、操作符、宏等]]"
down:
relation:
  - "[[变量声明与变量定义的区别]]"
---
使用`extern`来声明变量，适合在多个源文件时使用，可以在任何位置声明，但是只能定义一次

```C
\#include <iostream>
using namespace std;

// Variable declaration:
extern int a, b;
extern int c;
extern float f;

int main () {
   // Variable definition:
   int a, b;
   int c;
   float f;

   // actual initialization
   a = 10;
   b = 20;
   c = a + b;

   cout << c << endl ;

   f = 70.0/3.0;
   cout << f << endl ;

   return 0;
}
```

- 声明而不是定义外部变量，extern int a告诉编译器，有一个int类型的变量a定义在其他地方，如果有调用请去其他文件中查找定义。

> extern "C"

- 为了能够正确实现C代码调用其他C语言代码。加上extern “C”后，会指示编译器这部分代码按C语言（而不是C）的方式进行编译。由于C++支持函数重载，因此编译器编译函数的过程中会将函数的参数类型也加到编译后的代码中，而不仅仅是函数名其中反应了函数的参数和返回类型；而C语言并不支持函数重载，因此编译C语言代码的函数时不会带上函数的参数类型，一般只包括函数名
    - C++代码调用C语言代码
    - 在C++的头文件中使用
    - 在多个人协同开发时，可能有的人比较擅长C语言，而有的人擅长C++，这样的情况下也会有用到

```C
\#ifdef __cplusplus
extern "C" {
\#endif

void myFunction();

\#ifdef __cplusplus
}
\#endif
```
