---
tags:
  - C++
up:
  - "[[指针和引用]]"
  - "[[指针]]"
down:
relation:
  - "[[空指针、野指针、悬挂指针]]"
  - "[[指针]]"
  - "[[常量：define、const]]"
  - "[[常引用：const]]"
---
- const修饰指针

```C
// 修饰*p,也就是*p不可变，p指向的地址内容不可变，p的指向可以变，但是指向哪个哪个就不能变
	const int*p=&a；
	int const  *p=&a；
//修饰p,也就是p的指向不可变，指向地址的内容可以变
	int*const p=&a；
//*p和p都被修饰了，p的指向和指向地址的内容都不能变
	const int*const p=&a；
```
