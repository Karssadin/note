---
tags:
  - C++
  - STL
up:
  - "[[容器、适配器、工具]]"
down:
relation:
  - "[[unordered_map-unordered_multimap]]"
---
- (仅次于vector、list)——高性能、高效率
- 基于平衡二叉树（红黑树）
- map中所有元素都是pair
- pair中第一个元素为key(键值)，起到索引作用，第二个元素为value(实值)
- 所有元素都会根据元素的键值自动排序
- map和multimap属于`关联式容器`，底层结构是用二叉树实现
- 可以根据key值快速找到value值
- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素

## 常用函数、迭代器

- `find(key);`找key是否存在,若存在，返回该键的元素的迭代器;若不存在，返回map.end();
- `lower_bound(x)`返回>=x的最小数的迭代器
- `upper_bount(x)`返回>x的最下数的迭代器

### 构造和赋值

- 构造:
    - `map<T1，T2>mp;`默认构造函数:
    - `map(const map &mp);`拷贝构造函数
- 赋值:
    - `map& operator=(const map &mp);`重载等号操作符插入和遍历要使用pair的操作
- 遍历时候与其他容器一致，但是遍历时候要进行first、second处理

### 大小和交换

- `size();` 返回容器中元素的数目
- `emptyo;` 判断容器是否为空
- `swap(st);` 交换两个集合容器

### 插入和删除

- `inser(elem);`在容器中插入元素。
- `clear();` 清除所有元素
- `erase(pos);`删除pos迭代器所指的元素，返回下一个元素的迭代器。
- `erase(beg, end);`删除区间[beg,end)的所有元素，返回下一个元素的迭代器。
- `erase(key);`删除容器中值为key的元素。
- **不允许重复插入，不是覆盖，而是不进行插入！！**

```C
pair<type,type>(value1, value2);`
make_pair( value1，value2 );
map<int,int>::value_type(3,30);
m[4]=40;//时间复杂度o(log n)
//如果，使用第四种方法，key值没有对应的存储，会创建一个value为0的键值对
```

### 查找和统计

- `find(key);`找key是否存在,若存在，返回该键的元素的迭代器;若不存在，返回map.end();
- `count(key);`统计key的元素个数

### 排序，利用仿函数改变排序规则

默认是key值从小到大排序

```C
class Mycompare{
public :
	bool operator()(int v1,int v2){
		return v1>v2;
	}
};
std::map<int,int,Mycompare> m1;
```

### find 和 [] 的区别

- `[]`：如果 key 不存在，会自动插入一个具有该 key 和 value 默认值的元素
- `find()`：如果 key 不存在，返回 `end()` 迭代器，不会插入
- 只读访问时优先使用 `find()` 或 `count()`，避免意外插入

### 使用结构体作为 key

需要在结构体中重载 `<` 运算符，因为 map 底层红黑树需要比较大小来排序：

```cpp
struct Point {
    int x, y;
    bool operator<(const Point& other) const {
        return x < other.x || (x == other.x && y < other.y);
    }
};
std::map<Point, int> m;
```
