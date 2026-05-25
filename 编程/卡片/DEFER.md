---
tags:
  - C++
  - go
up:
  - "[[C++]]"
  - "[[RAII]]"
down:
relation:
  - "[[Go defer-panic-recover]]"
  - "[[异常处理]]"
  - "[[RAII]]"
---
# DEFER

C++ 中可以用 RAII 模拟 Go 的 `defer` 语义：构造对象时保存清理函数，在对象离开作用域析构时执行。

- [使用 C/C++ 模拟 defer 关键字 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/35191739)
- 该卡片讨论的是 C++ 模拟实现，Go 原生语义见 [[Go defer-panic-recover]]。

```C++
// 以下示例中，m_jobs-- 操作会在 threadFunc执行结束后执行。
// 若 doThreadJoin 执行过程中抛出异常，只要异常能被正常捕获吹了， m_jobs--也会正常执行
void Demo::threadFunc()
{
	DEFER([this]){
		m_jobs--;
	};
	doThreadJoin();
}
```
- 同一个作用域内可以有多个DEFER语句
- 当编译时带有参数 -fno-elide-constructors 时，由于产生临时变量会强制调用拷贝构造函数，导致 func 多次被调用，造成意料之外的结果。


```c++
#ifndef DEFER
#define UNIQUE_DEFER_IMPL_(x, y) x##y
#define UNIQUE_DEFER_(x) UNIQUE_DEFER_IMPL_(x, __COUNTER__)
#define DEFER(func) auto UNIQUE_DEFER_(_defer_) = defer_func(func)

template <typename DeferFunction>
class DeferOp
{
public:
	explicit DeferOp(DeferFunction func)
		: m_func(std::move(func))
	{
	}
	~DeferOp(){
		m_func();
	}
private:
	DeferFunction m_func;
};

template <typename DeferFunction>
DeferOp<DeferFunction> defer_func(DeferFunction f)
{
	return DeferOp<DeferFunction>(std::move(f));
}
```
