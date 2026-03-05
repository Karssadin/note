---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
  - "[[for_each：对序列中每个元素执行指定操作]]"
---
目标容器要进行提前开辟空间

- `transform(iterator beg1, iterator end1， iterator beg2，_func);`
    - beg1源容器开始迭代器
    - end1源容器结束迭代器
    - beg2目标容器开始迭代器
    - _func函数或者函数对象

```C
\#include <algorithm>
class Transform{
public:
	int operator()(int v){
		return v;
	}
};
int tran(int v){
	return v+100;
}
v2.resize(v1.size())
transform(v1.begin(),v1.end(),v2.begin(),tran);
transform(v1.begin(),v1.end(),v2.begin(),Transform());
```
