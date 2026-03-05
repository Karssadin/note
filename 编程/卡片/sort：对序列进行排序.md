---
tags:
  - STL
up:
  - "[[STL常用算法]]"
down:
relation:
  - "[[容器存储自定义数据类型指定排序规则]]"
  - "[[容器存储内置数据类型指定排序规则（利用仿函数）]]"
---
- `sort(iterator beg, iterator end，_Pred);`
    - `beg`：开始迭代器
    - `end`：结束迭代器
    - `_Pred`：谓词(不填默认为less关系仿函数)

```C
\#include <algorithm>
\#include <functional>
sort(v.begin(),v.end());//sort(v.begin(),v.end(),less<int>());
sort(v.begin(),v.end(),greater<int>());

//或者自己提供，仿函数，或者函数对象
```

```JavaScript
for(auto x: container)
	cout << x << endl;
for(auto i = a.begin(); i != a.end(); ++i)
	cout << *i << endl;
```

- 所有不支持随机访问迭代器的容器，不可以用标准算法sort，内部会提供非全局的算法函数
