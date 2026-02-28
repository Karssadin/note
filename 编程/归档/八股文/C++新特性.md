---
tags:
up:
  - "[[C++]]"
down:
  - "[[auto——自动类型推导]]"
  - "[[可变参数模板]]"
  - "[[constexpr函数]]"
relation:
---
# 早期
## [[RTTI]]

# 模板与编译期相关
## C++11
1. [[可变参数模板]]
2. [[constexpr函数]]
3. [[decltype——表达式类型推导]]
4. [[auto——自动类型推导]]
5. [[auto与decltype结合使用——返回类型后置语法的应用]]
6. static_assert——编译期断言
7. std::enable_if + SFINAE – 模板约束
8. 标准 type traits 库——is_same, is_integral, remove_reference
9. 内存对齐控制– alignas, alignof 


# C++11
## 

## 语言层增强
### [[右值引用]]
1. 左值引用
2. 右值引用
3. 左值和右值的区别
    1. 左值引用和右值引用的区别
4. 移动语义
5. 完美转发
    1. 实现完美转发
    2. 完美转发中去掉forward会怎么样
### [[智能指针]]
1. auto_ptr
2. unique_ptr
3. shared_ptr
4. weak_ptr
5. unique_ptr和shared_ptr的区别
### [[lambda]]

### 范围for循环

`for (char c : str)`
### [[nullptr]]

## 构造函数

1. 默认构造函数的 =delete 和 = default
    1. [[默认构造-析构函数]]
2. 移动构造函数和移动赋值函数的引入
    1. [[拷贝构造、移动构造、移动赋值]]
    2. [[拷贝构造函数与拷贝赋值和移动赋值的区别？]]
3. [[构造函数的初始化列表]]

## [[RAII]]


## [[DEFER]]

## 结构体和内存对齐：alignas

- 对齐限定符`**alignas**`

## C++14

## C++17

=================
要求对内容做非常详细的解释，比如它出来之前用什么，怎么用，它替代了什么，怎么使用，有什么特点、缺点、特殊使用技巧，14 17做了什么改进


## 1. 🧩 模板与编译期（Compile-time & Templates）

### ✅ C++11
    
- `constexpr` 函数（受限，不能写 if/for/switch）
    
- `decltype` – 表达式类型推导
    
- `auto` – 自动类型推导
    
- `static_assert` – 编译期断言
    
- `std::enable_if` + **SFINAE** – 模板约束
    
- 标准 **type traits** 库 – `is_same`, `is_integral`, `remove_reference` …
    
- **对齐控制** – `alignas`, `alignof`（编译期指令，影响内存布局）
    

### ✅ C++14

- 返回类型自动推导 – `auto f(T a, U b) { return a+b; }`
    
- `decltype(auto)` – 精确推导返回值
    
- 更强 `constexpr`（允许 if / for / switch 等语句）
    
- 变量模板 (Variable Templates) – `template <typename T> constexpr bool is_int_v = ...;`
    
- `std::integer_sequence` – 编译期整数序列工具
    

### ✅ C++17

- `if constexpr` – 编译期条件分支
    
- 折叠表达式 (Fold Expressions) – `(args + ...)`
    
- 类模板实参推导 (CTAD) – `std::pair p(1, 2.0)`
    
- `inline` 变量 – 避免模板 ODR 问题
    
- `std::void_t` – 标准化 SFINAE idiom
    
- `constexpr lambda` – Lambda 可以用于编译期
    

---

## 2. 🗂 类型与内存管理（Types & Memory）

### ✅ C++11

- 智能指针 – `unique_ptr`, `shared_ptr`, `weak_ptr`
    
- `nullptr` – 类型安全空指针
    
- RAII 强化 – 借助智能指针
    
- 对齐控制（交叉归类，见 1）
    

### ✅ C++14

- （无新增与内存直接相关的特性）
    

### ✅ C++17

- 超对齐支持（over-aligned allocation）
    
- `new/delete` 对齐规则增强
    

---

## 3. 🍬 语法糖与可读性（Syntactic Sugar）

### ✅ C++11

- Lambda 表达式（运行期函数对象，**非 constexpr**）
    
- 范围 for 循环 – `for (auto& x : container)`
    
- 委托构造函数 – 一个构造函数调用另一个
    
- 继承构造函数 – `using Base::Base`
    
- 默认构造函数/析构函数 `=default` / `=delete`
    
- 构造函数初始化列表改进
    

### ✅ C++14

- 泛型 Lambda – `[](auto x, auto y){...}`（仍是运行期）
    
- 二进制字面量 – `0b1010`
    
- 数字分隔符 – `1'000'000`
    

### ✅ C++17

- 结构化绑定 – `auto [x, y] = pair;`
    
- if / switch 初始化语法 – `if (auto it = m.find(x); it != m.end()) ...`
    

---

## 4. 🛠️ 语义与右值引用（Semantics & Rvalue References）

### ✅ C++11

- 右值引用 `T&&` – 移动语义 & 完美转发
    
- `std::move`, `std::forward` – 转发工具
    
- 移动构造函数 / 移动赋值运算符
    
    - [[拷贝构造、移动构造、移动赋值]]
        
    - [[拷贝构造函数与拷贝赋值和移动赋值的区别]]
        

### ✅ C++14

- （无新增）
    

### ✅ C++17

- 更强 `constexpr`（接近普通函数，结合右值引用更实用）
    

---

## 5. 🔧 其他语言增强（Other Enhancements）

### ✅ C++11

- 内联命名空间 – `inline namespace v1 {}`
    
- 属性语法 – `[[nodiscard]]`, `[[deprecated]]`
    
- `noexcept` – 异常说明
    

### ✅ C++14

- 属性改进 – `[[deprecated("reason")]]`
    

### ✅ C++17

- `[[maybe_unused]]` – 抑制未使用警告
    
- `[[nodiscard("reason")]]` – 更强的返回值检查
    
- `[[fallthrough]]` – switch 显式贯穿