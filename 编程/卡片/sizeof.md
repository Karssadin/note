---
tags: 
up: 
  - "[[关键字、修饰符、操作符、宏等]]"
  - "[[数据类型]]"
down: 
relation:
  - "[[基础数据类型]]"
  - "[[sizeof和strlen的区别]]"
---
- 统计数据类型所占内存大小是关键字不是函数
    - sizeof 对数组，得到整个数组所占空间大小。
    - sizeof 对指针，得到指针本身所占空间大小。

```C
\#include <iostream>
using namespace std;
int main() {
   cout << "Size of char : " << sizeof(char) << endl;
   cout << "Size of int : " << sizeof(int) << endl;
   cout << "Size of short int : " << sizeof(short int) << endl;
   cout << "Size of long int : " << sizeof(long int) << endl;
   cout << "Size of float : " << sizeof(float) << endl;
   cout << "Size of double : " << sizeof(double) << endl;
   cout << "Size of wchar_t : " << sizeof(wchar_t) << endl;

   return 0;
}
```