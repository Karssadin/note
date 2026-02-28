---
tags: 
up:
  - "[[STL]]"
down:
  - "[[STL优点]]"
  - "[[迭代器失效]]"
relation:
---
[[STL优点]]
[[迭代器失效]]

---
- `STL:Standard Template Library`标准模板库
---

> vector存放内置数据类型

- 容器：`vector`
- 算法：`foreach`
- 迭代器：`vector<int>::iterator`

```C++
vector<int> v;
	v.push_back(10);
	
	//遍历方法1
	vector<int>::iterator itBegin=v.begin();//指向容器中第一个元素
	vector<int>::iterator itEnd=v.end();//指向容器中最后一个元素的下一个位置
	if(itBegin!=itEnd){
		cout<<*(itBegin++)<<endl;
	}
	//遍历方法2
	for(vector<int>::iterator it=v.begin();it!=v.end();it++)
		cout<<*it<<endl;
	//遍历方法3,利用遍历算法
	for_each(v.begin(),v.end(),Myprint);//类似回调函数就是一个被作为参数传递的函数
	return 0;
```

> vector存放自定义数据类型

```C++
vector<Person> v;
	//迭代器it得到的是Person类型的指针，*解引用得到对应的数据，也可以使用->直接获得Person的数据
	//第三种遍历方法也类似，就是输出中添加Person.
```

> vector嵌套容器

```C++
vector<vector<int> >v;
	vector<int> v1
	vector<int> v3;
	v.push_back(v1);	
	v.push_back(v3);
	
	for(vector<vector<int>>::iterator it=v.begin();it!=v.end();it++){
		for(vector<int>::iterator vit=(*it).begin();vit!=(*it).end();vit++){
			cout<<*vi<<" ";
		}
	}
```