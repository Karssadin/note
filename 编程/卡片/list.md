---
tags:
up:
  - "[[容器、适配器、工具]]"
down:
relation:
---
- [[#构造函数|构造函数]]
- [[#赋值和交换|赋值和交换]]
- [[#大小操作|大小操作]]
- [[#插入和删除|插入和删除]]
- [[#数据存取|数据存取]]
- [[#反转和排序|反转和排序]]

----

## 底层原理

- 底层：双向链表，每个节点包含数据 + prev 指针 + next 指针
- 不支持随机访问（不能 `[]`），任意位置插入删除 O(1)
- 迭代器类型：双向迭代器（只能 `++`/`--`，不支持 `+n`）
- 迭代器失效：仅删除的节点失效，其他迭代器不受影响
- 动态分配存储，不会造成内存浪费和溢出，但遍历速度慢、占用空间大（每节点额外两个指针）
- 双向循环链表，一个指向prev、一个指向next
- list的迭代器只支持前移和后移，属于双向迭代器
- 插入和删除的操作都不会使得该迭代器的实现，因为指向的空间不变

### 构造函数

- `list<T> lst;` list采用采用模板类实现,对象的默认构造形式:
- `list(beg,end) ;` 构造函数将[beg, end)区间中的元素拷贝给本身。
- `list(n,elem);` 构造函数将n个elem拷贝给本身。
- `list(const list &lst);` 拷贝构造函数

### 赋值和交换

- `assign(beg, end);` 将[beg, end)区间中的数据拷贝赋值给本身。
- `assign(n,elem);` 将n个elem拷贝赋值给本身。
- `list& operator=(const list &lst);` 重载等号操作符
- `swap(lst);` 将lst与本身的元素互换

### 大小操作

- `size();` 返回容器中元素的个数
- `empty();` 判断容器是否为空
- `resize(num);`
    - 重新指定容器的长度为num，若容器变长，则以默认值填充新位置。
    - 如果容器变短，则末尾超出容器长度的元素被删除。
- `resize(num,elem);`
    - 重新指定容器的长度为num，若容器变长，则以elem值填充新位置
    - 如果容器变短，则末尾超出容器长度的元素被删除

### 插入和删除

- `push_back(elem);`在容器尾部加入一个元素
- `pop_back();`删除容器中最后一个元素
- `push_front(elem);`在容器开头插入一个元素
- `pop_front();`从容器开头移除第一个元素
- `insert(pos,elem);`在pos位置插elem元素的拷贝，返回新数据的位置。-
- `insert(pos,n,elem);`在pos位置插入n个elem数据，无返回值。
- `insert(pos,beg,end);`在pos位置插入[beg,end)区间的数据，无返回值。
- `clear();`移除容器的所有数据
- `erase(beg,end);`删除[beg,end)区间的数据，返回下一个数据的位置。
- `erase(pos);`删除pos位置的数据，返回下一个数据的位置。
- `remove(elem);`删除容器中所有与elem值匹配的元素。

### 数据存取

- `front();` 返回第一个元素。
- `back();` 返回最后一个元素。

### 反转和排序

- `reversel);`反转链表
- `sort();` 链表排序 可以提供一个函数进行降序排序，默认为升序操作
- 如果操作自定义数据类型的话，就需要进行自定义回调函数高级排序只是在排序规则上再进行一次逻辑规则制定，并不复杂

```C
//年龄升序，身高降序
bool comparePerson(Person &p1,Person &p2){
	if( p1.m_Age == p2.m_Age )
		return p1.m_Height > p2.m_Height;
	return p1.m_Age < p2.m_Age;
}

l1.sort(comparePerson)
```
