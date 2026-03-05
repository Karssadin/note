---
tags:
  - C++
up:
  - "[[虚函数与虚函数表]]"
  - "[[继承]]"
down:
relation:
  - "[[纯虚函数、抽象类、接口类]]"
  - "[[虚析构函数]]"
---

C++11 引入 `override` 和 `final` 两个**上下文关键字**（contextual keyword，仅在特定位置有关键字语义，可作为普通标识符使用）。

## override

**作用**：显式声明该成员函数是对基类虚函数的重写，让编译器帮助验证。

```cpp
class Base {
    virtual void foo(int x);
    virtual void bar();
};

class Derived : public Base {
    void foo(int x) override;  // ✅ 正确重写
    void bar() const override; // ❌ 编译错误：签名不匹配（加了 const）
    void baz() override;       // ❌ 编译错误：基类没有 baz 虚函数
};
```

**为什么需要 override：**
- 没有 `override` 时，若函数签名与基类不完全匹配（如参数类型、const 限定符有差异），会**静默地创建新函数**而不是重写，是常见 bug
- 加上 `override` 后，编译器强制检查签名一致性

```cpp
// 常见 bug（没有 override）
class Shape {
    virtual double area() const = 0;
};
class Circle : public Shape {
    double area() { return 3.14 * r * r; }  // 漏了 const，不是重写！
};

// 用 override 立即发现
class Circle : public Shape {
    double area() override { ... }  // ❌ 编译错误：缺少 const
    double area() const override { ... }  // ✅ 正确
};
```

## final

**作用**：禁止类被继承，或禁止虚函数被进一步重写。

### 修饰类（禁止继承）

```cpp
class Singleton final {   // 不能被继承
    ...
};

class Derived : public Singleton {};  // ❌ 编译错误
```

**应用场景：**
- 单例类（防止子类破坏单例约束）
- 性能优化（编译器可以对 final 类的虚函数调用进行**去虚化/内联优化**）

### 修饰虚函数（禁止进一步重写）

```cpp
class Base {
    virtual void foo();
};

class Middle : public Base {
    void foo() override final;  // foo 在此封闭，子类不能再重写
};

class Derived : public Middle {
    void foo() override;  // ❌ 编译错误：foo 是 final 的
};
```

## override 与 final 的位置

```cpp
class A {
    virtual void f() const override;   // override：放在 const 之后，函数体之前
    virtual void g() final;             // final：同位置
    virtual void h() const override final;  // 两者可组合
};
```

## 编译器去虚化优化

对 `final` 函数或 `final` 类，编译器可以：
1. 直接静态绑定虚函数调用（消除虚表查找开销）
2. 将小函数内联展开

```cpp
// 通常无法内联（需虚表查找）
void callFoo(Base* p) { p->foo(); }

// 对 final 类/函数，编译器可优化为直接调用
void callFoo(Final* p) { p->foo(); }  // 可以内联
```

## 最佳实践

- 所有重写的虚函数都应该加 `override`
- 叶子类（不打算被继承的类）考虑加 `final`
- 不需要进一步特化的虚函数考虑加 `final`
- `override` 使代码意图更清晰，是 C++ Core Guidelines 推荐做法
