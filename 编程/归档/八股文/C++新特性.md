---
tags:
up:
  - "[[C++]]"
down:
  - "[[RTTI]]"
  - "[[可变参数模板]]"
  - "[[constexpr函数]]"
  - "[[decltype——表达式类型推导]]"
  - "[[auto——自动类型推导]]"
  - "[[auto与decltype结合使用——返回类型后置语法的应用]]"
  - "[[type_traits]]"
  - "[[static_assert]]"
  - "[[SFINAE与enable_if]]"
  - "[[alignas与alignof]]"
  - "[[右值引用]]"
  - "[[智能指针]]"
  - "[[lambda]]"
  - "[[范围for循环]]"
  - "[[nullptr]]"
  - "[[noexcept]]"
  - "[[委托与继承构造函数]]"
  - "[[默认构造-析构函数]]"
  - "[[拷贝构造、移动构造、移动赋值]]"
  - "[[构造函数的初始化列表]]"
  - "[[RAII]]"
  - "[[DEFER]]"
  - "[[返回类型自动推导]]"
  - "[[decltype auto]]"
  - "[[泛型Lambda]]"
  - "[[变量模板]]"
  - "[[std--make_unique]]"
  - "[[二进制字面量与数字分隔符]]"
  - "[[std--integer_sequence]]"
  - "[[constexpr常量]]"
  - "[[结构化绑定]]"
  - "[[if constexpr]]"
  - "[[折叠表达式]]"
  - "[[类模板实参推导（CTAD）]]"
  - "[[std--optional]]"
  - "[[std--variant]]"
  - "[[std--any]]"
  - "[[std--string_view]]"
  - "[[std--filesystem]]"
  - "[[inline变量]]"
  - "[[constexpr lambda]]"
  - "[[if-switch初始化语法]]"
relation:
---
# 早期

1. [[RTTI]]

# C++11

## 类型推导与编译期

1. [[auto——自动类型推导]]
2. [[decltype——表达式类型推导]]
3. [[auto与decltype结合使用——返回类型后置语法的应用]]
4. [[constexpr函数]]：仅支持单条 return（C++14 大幅增强）
5. [[constexpr常量]]
6. [[static_assert]]：编译期断言
7. [[type_traits]]：标准类型特征库（is_same, is_integral, remove_reference…）
8. [[SFINAE与enable_if]]：模板约束机制
9. [[alignas与alignof]]：内存对齐控制

## 模板

1. [[可变参数模板]]：`template <typename... Args>`

## 语言层增强

1. [[右值引用]]
	1. 左值引用与右值引用的区别
	2. 移动语义：`std::move`
	3. 完美转发：`std::forward`
2. [[智能指针]]：unique_ptr、shared_ptr、weak_ptr
3. [[lambda]]：匿名函数对象
4. [[范围for循环]]：`for (auto& x : container)`
5. [[nullptr]]：类型安全的空指针
6. [[noexcept]]：异常规格说明

## 构造与析构

1. [[默认构造-析构函数]]：`=default` / `=delete`
2. [[委托与继承构造函数]]
3. [[拷贝构造、移动构造、移动赋值]]
	1. [[拷贝构造函数与拷贝赋值和移动赋值的区别？]]
4. [[构造函数的初始化列表]]

## 资源管理

1. [[RAII]]
2. [[DEFER]]

# C++14

## 类型推导增强

1. [[返回类型自动推导]]：函数可省略尾置返回类型
2. [[decltype auto]]：精确保留值类别，用于泛型转发

## 编译期增强

3. 更强 [[constexpr函数]]：允许 if / for / switch，函数体可包含局部变量
4. [[变量模板]]：`template <typename T> constexpr T pi = T(3.14);`
5. [[std--integer_sequence]]：编译期整数序列，展开 tuple

## 语言层增强

6. [[泛型Lambda]]：参数可用 `auto`
7. [[std--make_unique]]：补齐工厂函数，避免裸 new
8. [[二进制字面量与数字分隔符]]：`0b1010`、`1'000'000`

# C++17

## 编译期增强

1. [[if constexpr]]：编译期分支，替代 SFINAE
2. [[折叠表达式]]：`(args + ...)` 简化参数包展开
3. [[类模板实参推导（CTAD）]]：`std::pair p(1, 2.0)` 自动推导
4. [[constexpr lambda]]：Lambda 可用于编译期求值
5. [[inline变量]]：头文件中定义变量不违反 ODR

## 标准库新增

6. [[std--optional]]：安全表示"可能无值"
7. [[std--variant]]：类型安全的 union
8. [[std--any]]：类型擦除容器
9. [[std--string_view]]：零拷贝字符串视图
10. [[std--filesystem]]：跨平台文件系统操作库

## 语法增强

11. [[结构化绑定]]：`auto [x, y, z] = tuple;` 直接解包
12. [[if-switch初始化语法]]：`if (auto it = m.find(k); it != m.end())`
