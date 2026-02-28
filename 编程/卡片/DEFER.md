---
tags: 
up:
  - "[[C++]]"
down: 
relation:
---
- [使用 C/C++ 模拟 defer 关键字 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/35191739)
- C++中实现 `golang` 的DEFER语句，在当前作用域结束时执行目标函数。利用出作用域之后会调用对象的析构函数来实现相应逻辑

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
clasee DeferOp
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
}

template <typename DeferFunction>
DeferOp<DeferFunction> defer_func(DeferFunction f)
{
	return DeferOp<DeferFunction>(std::move(f));
}
```