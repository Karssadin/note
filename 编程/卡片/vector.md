---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
---
- [[#常用函数、迭代器|常用函数、迭代器]]
- [[#构造函数|构造函数]]
- [[#赋值操作|赋值操作]]
- [[#容量和大小|容量和大小]]
- [[#插入和删除|插入和删除]]
- [[#数据存取|数据存取]]
- [[#互换容器（利用匿名对象来收缩vector的空间）|互换容器（利用匿名对象来收缩vector的空间）]]
- [[#预留空间|预留空间]]

----
- 类似数组，单端数组（在尾端做插入删除）
- 数组是**静态空间**，`vector`可以**动态扩展**(并不是在原空间之后续接新空间，而是找更大的内存空间，然后将原数据拷贝新空间，释放原空间)
- `vector` 的迭代器支持随机访问的迭代器
- 支持比较运算

```C++
\#include <algorithm>
\#include <iostream>
\#include <vector>

class Point {
public:
  Point() : x_(0), y_(0) {
    std::cout << "Default constructor for the Point class is called.\n";
  }

  Point(int x, int y) : x_(x), y_(y) {
    std::cout << "Custom constructor for the Point class is called.\n";
  }

  inline int GetX() const { return x_; }
  inline int GetY() const { return y_; }
  inline void SetX(int x) { x_ = x; }
  inline void SetY(int y) { y_ = y; }
  void PrintPoint() const {
    std::cout << "Point value is (" << x_ << ", " << y_ << ")\n";
  }

private:
  int x_;
  int y_;
};

// A utility function to print the elements of an int vector. The code for this
// should be understandable and similar to the code iterating through elements
// of a vector in the main function.
void print_int_vector(const std::vector<int> &vec) {
  for (const int &elem : vec) {
    std::cout << elem << " ";
  }
  std::cout << "\n";
}

int main() {
  std::vector<Point> point_vector;

  // 初始化列表初始vector
  std::vector<int> int_vector = {0, 1, 2, 3, 4, 5, 6};

  // push_back 和 emplace_backe两个插入方法，emplace_back稍微快一些，因为emplace_back直接把参数给构造函数，在对应位置，emplace_back is slightly faster, since it forwards 
  // the constructor arguments to the object's constructor and constructs the object 
  // in place, while push_back constructs the object, thenmoves it to the memory 
  // in the vector.
  std::cout << "Appending to the point_vector via push_back:\n";
  point_vector.push_back(Point(35, 36));
  std::cout << "Appending to the point_vector via emplace_back:\n";
  point_vector.emplace_back(37, 38);

  // Let's just add more items to the back of our point_vector.
  point_vector.emplace_back(39, 40);
  point_vector.emplace_back(41, 42);

  // There are many ways to iterate through a vector. For instance, we can
  // iterate through it's indices via the following for loop. Note that it is
  // good practice to use an unsigned int type for array or vector indexes.
  std::cout << "Printing the items in point_vector:\n";
  for (size_t i = 0; i < point_vector.size(); ++i) {
    point_vector[i].PrintPoint();
  }

  // We can also iterate through it via a for-each loop. Note that I use
  // references to iterate through it so that the items we iterate through are
  // the items in the original vector. If we iterate through references of the
  // vector elements, we can also modify the data in the vector.
  for (Point &item : point_vector) {
    item.SetY(445);
  }

  // Let's see if our changes went through. Note that I use the const reference
  // syntax to ensure that the data I'm accessing is read only.
  for (const Point &item : point_vector) {
    item.PrintPoint();
  }

  // Now, we show how to erase elements from a vector. First, we can erase
  // elements by their position via the erase function. For instance, if we want
  // to delete int_vector[2], we can call the following function with the
  // following arguments. The argument passed into this erase function has
  // the type std::vector<int>::iterator. An iterator for a C++ STL container
  // is an object that points to an element within the container. For instance,
  // int_vector.begin() is an iterator object that points to the first element
  // in the vector. The vector iterator also has a plus operator that takes
  // a vector iterator and an integer. The plus operator will increase the 
  // index of the element that the iterator is pointing to by the number passed
  // in. Therefore, int_vector.begin() + 2 is pointing to the third element in
  // the vector, or the element at int_vector[2].
  // If you are confused about iterators, it may be helpful to read the header of
  // iterator.cpp.
  int_vector.erase(int_vector.begin() + 2);
  std::cout << "Printing the elements of int_vector after erasing "
               "int_vector[2] (which is 2)\n";
  print_int_vector(int_vector);

  // We can also erase elements in a range via the erase function. If we want to
  // delete elements starting from index 1 to the end of the array, then we can
  // do so the following. Note that int_vector.end() is an iterator pointing to
  // the end of the vector. It does not point to the last valid index of the
  // vector. It points to the end of a vector and cannot be accessed for data.
  int_vector.erase(int_vector.begin() + 1, int_vector.end());
  std::cout << "Printing the elements of int_vector after erasing all elements "
               "from index 1 through the end\n";
  print_int_vector(int_vector);

  // We can also erase values via filtering, i.e. erasing values if they meet a
  // conditional. We can do so by importing another library, the algorithm
  // library, which gives us the std::remove_if function, which removes all
  // elements meeting a conditional from an iterator range. This does seem
  // awfully complicated, but the code can be summarized as follows.
  // std::remove_if takes in 3 arguments. Two of those arguments indicate the
  // range of elements that we should filter. These are given by
  // point_vector.begin() and point_vector.end(), which are iterators that point
  // to the beginning and the end of a vector respectively. Therefore, when we
  // pass these in, we are implying that we want the whole vector filtered.
  // The third argument is a conditional lambda type (see the std::function
  // library in C++, or at 
  // https://en.cppreference.com/w/cpp/utility/functional/function), that takes
  // in one argument, which is supposed to represent each element in the vector
  // that we are filtering. This function should return a boolean that is true
  // if the element is to be filtered out and false otherwise. std::remove_if
  // returns an iterator pointing to the first element in the container that
  // should be eliminated. Keep in mind that it swaps elements as needed,
  // partitioning the elements that need to be deleted after the iterator value
  // it returns. When erase is called, it deletes only the elements that
  // remove_if has partitioned away to be deleted, up to the end of the vector.
  // This outer erase takes a range argument, as we saw in the previous example.
  point_vector.erase(
      std::remove_if(point_vector.begin(), point_vector.end(),
                     [](const Point &point) { return point.GetX() == 37; }),
      point_vector.end());

  // After calling remove here, we should see that three elements remain in our
  // point vector. Only the one with value (37, 445) is deleted.
  std::cout << "Printing the point_vector after (37, 445) is erased:\n";
  for (const Point &item : point_vector) {
    item.PrintPoint();
  }

  // We discuss more stylistic and readable ways of iterating through C++ STL
  // containers in auto.cpp! Check it out if you are interested.

  return 0;
}
```

### 常用函数、迭代器

- `push_back()` `pop_back()`
- `begin（第一个数据）` `end(最后一个数据下一个位置)` `rbegin(最后一个数据)` `rend(第一个数据前面一个位置)`

### 构造函数

- `vector<T> v;` 采用模板实现类实现，默认构造函数 常用
- `vector(v.begin(), v.end());` 将v[begin(), end())区间中的元素拷贝给本身。
- `vector(n, elem);` 构造函数将n个elem拷贝给本身。
- `vector(const vector &vec);` 拷贝构造函数 常用

### 赋值操作

- `vector& operator=( const vector &vec);` 重载等号操作符
- `assign(beg,end);` 将[beg, end)区间中的数据拷贝赋值给本身。
- `assign(n, elem);` 将n个elem拷贝赋值给本身。
- 与构造函数类似，但是是后操作

### 容量和大小

- `empty();` 判断容器是否为空
- `capacity();` 容器的容量
- `size();` 返回容器中元素的个数 个数<=容量
- `resize(int num) ;`
    - 重新指定容器的长度为num，若容器变长，则以默认值0填充新位置。
    - 如果容器变短，则末尾超出容器长度的元素被删除。
- `resize(int num，elem);`
    - 重新指定容器的长度为num，若容器变长，则以elem值填充新位置。
    - 如果容器变短，则末尾超出容器长度的元素被删除如果一个vector使用默认的capacity，那么在push_back操作的时候，会根据添加元素的数量，动态的自动分配空间，2^n递增如果声明vector的时候，显式的使用capacity(size_type n)来指定vector的容量，那么在push_back的过程中（元素数量不超过n），vector不会自动分配空间。
- 使用capacity指定vector的容量为n，当push_back的元素数量大于n的时候，会重新分配一个大小为`2^m`的新空间，再将原有的n的元素和新的元素放入新开辟的内存空间中。（注：重新分配内存，并不会在原有的地址之后紧跟着分配的新的空间，一般会重新开辟一段更大的空间，再将原来的数据和新的数据放入新的空间)

### 插入和删除

- `push_back(ele);` 尾部插入元素ele
- `push_back`接受已构造的元素，执行拷贝或移动操作。
- `emplace_back`直接在向量中构造元素，通过参数传递构造所需的参数。
- `pop_back();` 删除最后一个元素
- `insert(const_iterator pos, ele);` `迭代器`指向位置pos插入元素ele
- `insert(const_iterator pos,int count,ele);` `迭代器`指向位置pos插入count个元素
- `eleerase(const_iterator pos) ;` 删除`迭代器`指向的元素
- `erase(const_iterator start，const_iterator end);`删除`迭代器`从start到end之间的元素
- `clear();` 删除容器中所有元素

### 数据存取

- `at(int idx);` 返回索idx所指的数据
- `operator[idx];` 返回索idx所指的数据
- `front();` 返回容器中第一个数据元素
- `back();` 返回容器中最后一个数据元素
- 也可以通过这些方法进行修改对应位置的值，类似string

### 互换容器（利用匿名对象来收缩vector的空间）

- `swap(vec);`将vec与本身的元素互换
    - 使用swap可以收缩内存空间，如果创建时候，没有指定，容量在之后的push_back可能会变大，之后使用resize之后，个数变小，但是容量不变

```C
// 比如v 的capacity是128，size是100
v.resize(3);
//v 的capacity是128，3
//使用swap来收缩内存
vector<int> (v).swap(v);
//采用匿名对象vector<int>() 但是用v初始化vector<int>(v)
//该匿名对象就是v的内容，但是只有3个capacity，和3个size，之后将其与V进行交换swap，V的空间就收缩为3，该匿名对象就回收了
```

### 预留空间

- 减少vector在动态扩展容量时的扩展次数
- `reserve(int len);` 容器预留len个元素长度，预留位置不初始化，元素不可访问，之后如果在动态扩展时候，是按len*2^n 来操作的
- resize或者默认扩展的空间中的预留位置是初始化为0，可以访问，这里的地方不可以访问