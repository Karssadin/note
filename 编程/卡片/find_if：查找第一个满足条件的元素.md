---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
  - "[[find：查找第一个等于给定值的元素（自定义类型需要重载==）]]"
  - "[[adjacent_find：查找相邻的重复元素]]"
---
- 按条件查找元素，找到返回对应位置迭代器，找不到返回结束迭代器`end()`
- `find_if(iterator beg, iterator end，_Pred ) ;`
    - beg开始迭代器
    - end结束迭代器
    - _Pred 函数或者谓词(返回bool类型的仿函数)

```C
class GreaterFive{
public:
	bool operator(int val)
		return val>5;
};
//查找容器中有没有大于5的数字
find_if(v.begin(),v.end(),GreaterFive());//创建匿名函数对象
//或者使用greater，functional中的函数对象

//自定义的class，也需要定义匿名对象
```
