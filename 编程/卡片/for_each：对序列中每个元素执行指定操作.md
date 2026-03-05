---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
  - "[[transform：对序列中每个元素执行指定操作并将结果存储到另一个序列中]]"
---
- `for_each(iterator beg, iterator end，_func);`
    - beg开始迭代器
    - end结束迭代器
    - _func函数或者函数对象，普通函数，仿函数

```C
template <class _InIt, class _Fn>
_Fn for_each(_InIt _First, _InIt _Last, _Fn _Func) {
// perform function for each element [_First, _Last)
    _Adl_verify_range(_First, _Last);
    auto _UFirst      = _Get_unwrapped(_First);
    const auto _ULast = _Get_unwrapped(_Last);
    for (; _UFirst != _ULast; ++_UFirst) {
        _Func(*_UFirst);
    }

    return _Func;
}
```

```C
\#include <algorithm>
void print(int &val){
	std::cout<<val;
}
class Print01{
public:
	void operator()(int val){
		std::cout<<val;
	}
};
for_each(v.begin(),v.end(),print)；
for_each(v.begin(),v.end(),Print01())
```
