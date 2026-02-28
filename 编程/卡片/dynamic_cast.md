---
tags:
up:
  - "[[类型转换]]"
down:
relation:
---

- `dynamic_cast (expression)`
- 进行下行转换时，dynamic_cast具有类型检查（信息在虚函数中）的功能，比static_cast更安全。
- 转换后必须是类的指针、引用或者void*，基类要有虚函数，可以交叉转换。
- dynamic本身只能用于存在虚函数的父子关系的强制类型转换；对于指针，转换失败则返回nullptr，对于引用，转换失败会抛出异常

```C
class Base {};
class Derived : public Base {};
Base* basePtr = new Derived();
Derived* derivedPtr = dynamic_cast<Derived*>(basePtr); // 安全的下行转换
```
