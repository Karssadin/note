---
tags:
  - C++
up:
  - "[[C++]]"
  - "[[C++新特性]]"
down:
relation:
  - "[[类型转换]]"
  - "[[查看类型：typeid()]]"
  - "[[dynamic_cast]]"
---

**RTTI（Runtime Type Identification）**：运行时类型识别，C++ 提供的在运行期获取对象动态类型的机制。

## 三个组成部分

### 1. `typeid` 运算符

返回表达式的**实际运行时类型**信息（对多态类型）。

```cpp
#include <typeinfo>

class Base { virtual void f() {} };
class Derived : public Base {};

Base* p = new Derived();
std::cout << typeid(*p).name() << std::endl;  // 输出 Derived 的 mangled name

// 非多态类型：编译期确定，不走虚表
int x = 42;
std::cout << typeid(x).name() << std::endl;   // 输出 "i"（int）
```

**注意**：
- 对多态类型（有虚函数的类）解引用指针，`typeid` 返回**动态类型**
- 对非多态类型，`typeid` 在**编译期**确定
- `name()` 返回的字符串是实现相关的（mangled name），用 `c++filt` 解码
- 对空指针解引用会抛出 `std::bad_typeid`

### 2. `dynamic_cast` 运算符

在继承层次中进行**安全的向下转型**（downcast）。

```cpp
Base* base = new Derived();

// 指针转型：失败返回 nullptr
Derived* d = dynamic_cast<Derived*>(base);
if (d) {
    // 转型成功
} else {
    // 转型失败（base 实际不是 Derived）
}

// 引用转型：失败抛出 std::bad_cast
try {
    Derived& dr = dynamic_cast<Derived&>(*base);
} catch (const std::bad_cast& e) {
    // 处理转型失败
}
```

**要求**：基类必须至少有一个虚函数（多态类型），否则编译错误。

**与 `static_cast` 的区别**：
- `static_cast`：编译期，不做运行时检查，不安全
- `dynamic_cast`：运行时，有类型检查，安全但有开销（需遍历虚表）

### 3. `type_info` 类

`typeid` 返回 `const std::type_info&`，支持：
- `name()`：返回类型名（实现相关）
- `==` / `!=`：比较两个类型是否相同
- `before()`：规定类型的排序关系（用于关联容器）

```cpp
const std::type_info& t1 = typeid(int);
const std::type_info& t2 = typeid(double);
if (t1 != t2) { /* 不同类型 */ }
```

## 性能开销与注意事项

- `typeid` 和 `dynamic_cast` 需要访问对象的**虚表**，有运行时开销
- 编译时禁用 RTTI（GCC: `-fno-rtti`）可减少二进制大小，但 `dynamic_cast` 和 `typeid` 将不可用
- 过度使用 RTTI 通常意味着设计问题，应优先用**虚函数（多态）** 代替 `typeid` 的类型判断
- **避免用 RTTI 实现的条件分支**（违反开闭原则），改用虚函数

## 典型使用场景

```cpp
// ❌ 不推荐：用 typeid 替代多态
void process(Base* p) {
    if (typeid(*p) == typeid(Derived)) { /* ... */ }
}

// ✅ 推荐：用虚函数实现多态
class Base { virtual void process() = 0; };
class Derived : public Base { void process() override { /* ... */ } };

// ✅ dynamic_cast 的合理使用：安全降型访问派生类特有接口
void handleDerived(Base* p) {
    if (auto* d = dynamic_cast<Derived*>(p)) {
        d->derivedOnlyMethod();
    }
}
```
