---
tags:
  - C11
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[模板实例化与分支裁剪]]"
---
- 在constexpr出来之前，编译期常量计算依赖
1. 宏
	1. 可以在编译期替换文本，常量值直接嵌入代码。  
	2. 不类型安全，调试困难，不会做边界检查或作用域控制。
```C++
#define SQUARE(x) ((x)*(x))
```
2. 模板元编程（TMP, Template Meta Programming）
	1. 优点：类型安全，可以在编译期进行复杂计算
	2. 缺点：语法繁琐，可读性差，错误信息难懂
```C++
template<int N>
struct Square {
    static const int value = N * N;
};
int arr[Square<3>::value];

```


- `constexpr` 函数模板可以完全替代 TMP 类模板进行简单计算。
```C++
template<int N>
constexpr int square() { return N * N; }

int arr[square<3>()]; // ✅ 可以在编译期求值
//上面的N必须在编译期已知

//下面就不用
constexpr int square(int x) { return x*x; }
int n;
std::cin >> n;
int val = square(n); // 运行期计算

//也可以写成这种
template<typename T>
constexpr T square(T x) { return x * x; }
constexpr int a = square(5); // 编译期
int n = 3;
int b = square(n);           // 运行期

```

- 函数体只能有 **return 表达式**，不能有 if/for/switch 或局部变量。
    
- `constexpr` 函数可以在运行期调用，但如果参数是常量表达式，编译器会在编译期计算。
    
- 如果运行期调用且参数是变量，结果会在运行期求值。
- 
- 例如调用某个类型不支持的操作，会编译直接报错，即便某些路径永远不会执行。
## 后续优化

- C14:
	- 可以写局部变量、if、for、switch，逻辑更直观。   
	- 支持递归 `constexpr`（如阶乘、斐波那契）。
- C17：
	- 可以使用if constexpr来做条件裁减. 这个特性也可以用在模板特例化中，防止错误分支编译
```C++
template<typename T>
constexpr auto identity(T t) {
    if constexpr(std::is_integral_v<T>)
        return t; // 编译期裁剪分支
    else
        return t + 0.0;
}
```