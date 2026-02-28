---
tags: 
up: 
  - "[[内存管理]]"
down: 
relation:
  - "[[malloc、calloc、realloc、alloca]]"
  - "[[定位new运算符（placement new）]]"
  - "[[空指针、野指针、悬挂指针]]"
  - "[[重载new运算符]]"
---
- C++中利用new操作符在堆区开辟数据。
- 堆区开辟的数据，由程序员手动开辟，手动释放，释放利用操作符delete。
- 利用new创建的数据，会返回该数据对应的类型的指针

```C
//语法：new 数据类型
new  int(10);
delete p;

//allocate an array
void *operator new[](size_t);
new int[10]

//free an array
void *operator delete[](void *);
	delete [] p
```

---

- 分类：
    - new/delete是运算符
    - malloc/free是库函数`（\#include <cstdlib>）`.运算符可以重载
- 析构和构造：
    - new/delete除了申请内存还会调用对象的构造函数/析构函数
    - malloc/free则不会调用
- 分配大小：
    - 使用new操作符申请内存分配时无须指定内存块的大小（自动计算）
    - malloc则需要显式地指出所需内存的尺寸。
- 失败处理
    - new内存分配失败时，会抛出`bac_alloc`异常，它不会返回NULL，抛出异常，所以可以使用try...catch...代码块的方式：
    - malloc分配内存失败时返回NULL，对于malloc来说，判断其返回空指针则马上用return语句终止该函数或者exit终止该程序
- 返回类型
    - new返回适当类型的指针
    - malloc返回void *指针，malloc的返回值为void*，需要进行强制转换
- 类型安全：new/delte是类型安全的，而malloc/free不是

---

- delete和free被调用后，内存不会立即回收，指针也不会指向空，出现野指针/悬挂指针。需要指向nullptr
- delete对象数组，需要使用delete[]，无法使用free来释放对象数组，因为需要调用析构函数