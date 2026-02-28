---
标签:
  - C++
相关索引:
  - "[[STL]]"
  - "[[C++]]"
---
vector

list

vector和list的区别

deque——双端数组

pair容器

map

set

map和set的区别

unordered_map

map和unordered_map区别?

unordered_set

stack

queue

priority_queue

使用数组实现栈

仿函数

## vector

> vector的原理，怎么扩容？

---

- std::vector 在内部维护了三个主要的指针：
    - start: 指向数组的起始位置。
    - finish: 指向数组的最后一个元素之后的位置。
    - end_of_storage: 指向数组分配的存储空间的结尾。
- 当你在 std::vector 中添加元素时，元素会被放在 finish 指针当前指向的位置，并且 finish 会递增，以指向下一个可用位置。

---

- Vector在堆中分配了一段连续的内存空间来存放元素
- 堆中分配内存，元素连续存放，内存空间只动态增长不会减少。使用`shrink_to_fit()`
- 迭代器：
    - first ： 指向的是vector中对象的起始字节位置
    - end ： 指向当前最后一个元素的末尾字节

---

> 扩容

- 当在 std::vector 中添加一个元素，而当前的存储已经满了（即 finish 和 end_of_storage 指向同一个位置）时，std::vector 需要扩容。扩容的具体步骤如下：
    - std::vector 会分配一个新的、更大的内存块。这个新块的大小通常是当前大小的两倍，但这个增长策略可能因实现而异。
    - std::vector 将当前存储的元素从旧的内存块复制或移动到新的内存块。
    - 释放旧的内存块。
    - 更新 start, finish, 和 end_of_storage 以指向新的内存块和适当的位置。
- vector有两个函数，一个是capacity()，在不分配新内存下最多可以保存的元素个数，另一个size()，返回当前已经存储数据的个数
- 对于vector来说，capacity是永远大于等于size的。capacity和size相等时再进行insert，vector就会扩容，capacity变大（翻倍）
- 一次扩容 capacity 翻倍的方式使得正常情况下添加元素需要扩容的次数大大减少（预留空间较多），时间复杂度较低; 每次扩容空间翻倍，而很多空间没有利用上

---

- resize()和reserve()的区别?
    - resize()：改变当前容器内含有元素的数量(size())，而不是容器的容量
    - 当resize(len)中len>v.capacity()，则数组中的size和capacity均设置为len;
    - 当resize(len)中len<=v.capacity()，则数组中的size设置为len，而capacity不变;
    - reserve()：改变当前容器的最大容量（capacity）
    - 如果reserve(len)的值 > 当前的capacity()，那么会重新分配一块能存len个对象的空间，然后把之前的对象通过copy construtor复制过来，销毁之前的内存;
    - 当reserve(len)中len<=当前的capacity()，则数组中的capacity不变，size不变，即不对容器做任何改变

---

- 迭代器失效的情况？
    - 当插入一个元素到vector中，由于引起了内存重新分配，所以指向原内存的迭代器全部失效。
    - 当删除容器中一个元素后,该迭代器所指向的元素已经被删除，那么也造成迭代器失效。erase方法会返回下一个有效的迭代器，所以当我们要删除某个元素时，需要it=vec.erase(it);

---

- vector中的元素可以是引用吗？
    - vector的底层实现要求连续的对象排列，引用并非对象，没有实际地址，因此vector的元素类型不能是引用

---

> 收缩空间

```C
//使用swap来收缩内存
vector<int> (v).swap(v);
//采用匿名对象vector<int>() 但是用v初始化vector<int>(v)
//该匿名对象就是v的内容，但是只有3个capacity，和3个size，之后将其与V进行交换swap，V的空间就收缩为3，该匿名对象就回收了
```

---

- 常用操作：push_back, pop_back, at, begin, end, size, resize, capacity, reserve

### list

- list使用双向链表实现双向链表，其结点与list本身是分开设计的
- 每个节点包含数据和两个指针：指向前一个节点和后一个节点。
- 每个元素都是放在一块内存中，他的内存空间可以是不连续的，使用指针来进行数据的访问
- 插入和删除元素时，只需要改变相邻节点的指针，不需要移动其他节点。
- 常用来做随机插入和删除操作容器,list不支持随机存取，适合需要大量的插入和删除，而不关心随机存取的应用场景。

```C
template<class T, class Alloc = alloc>
class list {
protected:
    typedef listnode <T> listnode;
public:
    typedef listnode link_type;
    typedef listiterator<T, T &, T> iterator;
protected:
    link_type node;
};
template<class T>
struct _listnode {
    typedef void voidpointer;
    void_pointer prev;
    void_pointer next;
    T data;
};
```

---

- 常用操作：push_back, push_front, pop_back, pop_front, begin, end, size, remove, splice, merge

### vector和list的区别

- vector底层实现是数组；list是双向链表
- vector是顺序内存,支持随机访问，list不行
- vector在中间节点进行插入删除会导致内存拷贝，list不会
- vector一次性分配好内存，不够时才进行翻倍扩容；list每次插入新节点都会进行内存申请
- vector随机访问性能好，插入删除性能差；list随机访问性能差，插入删除性能好

### deque——双端数组

- 支持快速随机访问，由于deque需要处理内部跳转，因此访问速度上没有vector快，但deque支持高效的头部插入和删除
- 双端队列。内部由多个固定大小的数组块组成。有指向第一个块和最后一个块的指针。
- 可以在前端和后端都进行快速插入和删除。
- deque是一个双端开叉的连续线性空间，其内部为分段连续的空间组成，随时可以增加一段新的空间并链接
- 由于deque的迭代器比vector要复杂，这影响了各个运算层面，所以除非必要尽量使用vector；为了提高效率，在对deque进行排序操作的时候，我们可以先把deque复制到vector中再进行排序最后在复制回deque
- 如果要想在删除内容的同时释放内存，那么你可以选择deque容器。
- deque是由一段一段的定量连续空间构成。一旦有必要在其头端或者尾端增加新的空间，便配置一段定量连续空间，串接在整个deque的头端或者尾端
    - deque内部有个中控器，维护每段缓冲区中的内容，缓冲区中存放真实数据
    - 避免“vector的重新配置，复制，释放”的轮回，维护连整体连续的假象，并提供随机访问的接口；
    - 其迭代器变得很复杂： `front` `back` `begin` `end`。

---

- 常用操作：push_back, push_front, pop_back, pop_front, at, begin, end, size

### pair容器

- `pair<T1, T2> p`

```C
template <typename T1, typename T2>
struct pair {
    T1 first;
    T2 second;
};
```

- 在底层被定义为一个struct，其所有成员默认是public，两个成员分别是first和second
- 其中map的元素是pair，`pair<const key_type，mapped_type>`可以用来遍历，插入关联容器.插入返回`pair<迭代器, bool>`

### map

- map是一种关联容器，用于存储键值对（key-value pairs），其中每个键是唯一的。
- 原理：
    - 基于平衡二叉搜索树（通常是红黑树）。
    - 元素按键值进行排序。
    - 可以进行快速的查找、插入和删除操作。

---

- map中find和[]的区别
    - map的下标运算符[]的作用是：将关键码作为下标去执行查找，并返回对应的值；如果不存在这个关键码，就将一个具有该关键码和值类型的默认值的项插入这个map。
    - map的find函数：用关键码执行查找，找到了返回该位置的迭代器；如果不存在这个关键码，就返回尾迭代器。

---

- map可以使用结构体作为key吗？
    - 需要在结构体中重载 < 运算符

---

- map为什么不用avl树？
    - 旋转操作：
        - AVL树 对于每次插入或删除，可能需要多次的旋转操作来保持树的平衡。
        - 红黑树 通常最多只需要3次旋转（在某些情况下只需要1次或2次）来重新平衡。
        - 这意味着红黑树在实际操作中通常比 AVL 树有更低的常数因子开销。
    - 平衡性：
        - AVL树 提供了更为严格的平衡，因此它提供了更为稳定和预测的查找时间。
        - 红黑树 允许更大的平衡偏斜，但插入和删除通常更快。
    - 实现复杂性：
        - AVL树 由于其严格的平衡要求，其实现通常比红黑树更为复杂。
        - 红黑树 由于它的平衡条件相对宽松，实现起来更为简单。
    - 内存开销：AVL树需要存储平衡因子（例如高度差或其它信息），而红黑树只需要一个比特来存储节点的颜色（红或黑）。这使得红黑树在内存开销上更有优势。

---

- 常用操作：insert, find, erase, begin, end, size, operator[].

### set

- set是一种集合容器，用于存储唯一的元素
- 原理：
    - 基于平衡二叉搜索树（通常是红黑树）。
    - 存储的元素自动排序。

---

- 常用操作：insert, find, erase, begin, end, size.

### map和set的区别

- 都是C++的关联容器,只是通过它提供的接又对里面的元素进行访问，底层都是采用红黑树实现。
- 不同点：
    - set：用来判断某一个元素是不是在一个组里面。
    - map：映射，相当于字典，把一个值映射成另一个值，可以创建字典。
- 优点：查找某一个数的时间为O(logn)；遍历时采用iterator，效果不错。
- 缺点：每次插入值的时候，都需要调整红黑树，效率有一定影响。

---

- map和set为什么要成倍的扩容而不是一次增加一个固定大小的容量呢？
    - 采用成倍方式扩容，可以保证常数的时间复杂度，而增加指定大小的容量只能达到O(n)的时间复杂度。

---

- 为什么是以两倍的方式扩容而不是三倍四倍，或者其他方式呢
    - 考虑可能产生的堆空间浪费，所以增长倍数不能太大，一般是1.5或2；GCC是2；VS是1.5，k =2 每次扩展的新尺寸必然刚好大于之前分配的总和，之前分配的内存空间不可能被使用，这样对于缓存并不友好，采用1.5倍的增长方式可以更好的实现对内存的重复利用。
    - C++并没有规定扩容因子K，这是由标准库的实现者决定的

### unordered_map

- 原理：
    - 基于哈希表。
    - 使用一个哈希函数将键映射到桶。
    - 在同一个桶中的元素可以使用链表或其他数据结构进行组织。
    - 当负载因子（存储的元素数与桶数之比）达到特定值时，可能会重新哈希。

### map和unordered_map区别?

- 他们都不是线程安全的
- 底层数据结构：
    - map：基于平衡二叉搜索树（通常是红黑树）实现。
    - unordered_map：基于哈希表实现。
- 元素顺序：
    - map：元素根据键的顺序进行排序。这是由于其基于二叉搜索树的特性。
    - unordered_map：元素的顺序是不确定的，不会根据键的顺序进行排序。
- 查找、插入和删除的时间复杂度：
    - map：这些操作通常具有 O(log n) 的时间复杂度。
    - unordered_map：理论上，查找操作具有 O(1) 的平均时间复杂度，但在最坏情况下可能退化为 O(n)。插入和删除操作也具有类似的性能特性。
- 哈希函数：
    - map：不使用哈希函数，因为它是基于比较的。
    - unordered_map：使用哈希函数来映射键到桶。
- 内存使用：
    - map：由于树结构的特性，通常使用的内存稍多。
    - unordered_map：哈希表的存储开销可能会更大，尤其是在维护较低的负载因子时。
- 迭代器的稳定性：
    - map：迭代器在插入和删除操作中保持稳定（即，插入或删除一个元素不会使指向其他元素的迭代器失效）。
    - unordered_map：在某些情况下，如重新哈希，迭代器可能会失效。
- 用途：
    - map：当需要有序的键值对集合或频繁的查找操作时使用。
    - unordered_map：当元素的顺序不重要，并且需要快速的查找、插入、删除操作时使用。

---

- 常用操作：insert, find, erase, begin, end, size, bucket_count, load_factor.

### unordered_set

- 原理：
    - 基于哈希表，与std::unordered_map的原理类似，但只存储键。

---

- 常用操作：insert, find, erase, begin, end, size.

### stack

- 原理：
    - 后进先出 (LIFO) 数据结构。
    - 默认基于 std::deque 实现，但也可以基于 std::list 或 std::vector。

---

- 常用操作：push()、pop()、top()、empty()、size()

### queue

- 原理：
    - 先进先出 (FIFO) 数据结构。
    - 默认基于 std::deque 实现，但也可以基于 std::list。

---

- 常用操作：push()、pop()、front()、back()、empty()、size()

### priority_queue

- 其内的元素不是按照被推入的顺序排列，而是自动取元素的权值排列，确省情况下利用一个max-heap完成
- 不提供任何迭代器或遍历机制
- 原理：
    - 一个队列，其中每次从队首取出的元素是当前队列中权值最大的元素。
    - 基于堆 (通常是二叉堆) 实现，此堆通过 std::vector 进行管理，但也可以通过 std::deque 实现。

---

- 常用操作：push()、pop()、top()、empty()、size()

```C
std::priority_queue<int> maxHeap;//默认大根堆
std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
```

### 使用数组实现栈

```C
template <typename T, int MAX_SIZE>
class Stack {
private:
    T elems[MAX_SIZE];    // 使用数组存储元素
    int topIndex = -1;    // 初始化为-1，表示堆栈为空
public:
    bool empty() const {
        return topIndex == -1;
    }
    bool full() const {
        return topIndex == MAX_SIZE - 1;
    }
    int size() const {
        return topIndex + 1;
    }
    void push(const T& elem) {
        if (full()) {
            throw std::runtime_error("Stack is full!");
        }
        elems[++topIndex] = elem;
    }
    void pop() {
        if (empty()) {
            throw std::runtime_error("Stack is empty!");
        }
        --topIndex;
    }
    T& top() {
        if (empty()) {
            throw std::runtime_error("Stack is empty!");
        }
        return elems[topIndex];
    }
    const T& top() const {
        if (empty()) {
            throw std::runtime_error("Stack is empty!");
        }
        return elems[topIndex];
    }
    /*
    为一个成员函数提供const版本取决于这个函数是否需要在const上下文中使用，以及这个函数是否会修改对象的状态。如果一个函数只是检查对象的状态而不修改它，那么它通常应该是const的。
    */
};
```

### 仿函数

- 二元运算
    - `template<class T>T plus<T>` 加法仿函数
    - `template<class T> T minus<T>` 减法仿函数
    - `template<class T> T multiplies<T>` 乘法仿函数
    - `template<class T> T divides<T>` 除法仿函数
    - `template<class T>T modulus<T>` 取模仿函数
- 一元运算
    - `template<class T>T negate<T>`取反仿函数

```C
negate<int> n;
n(50);//-50
plus<int> p;//一个类型，无法定义两个类型，默认传的为同种类型
p(10,10);//20
```

- `template<class T> bool equal_to<T>` 等于
- `template<class T> bool not_equal_to<T>` 不等于
- `template<class T> bool greater<T>` **大于**
- `template<class T> bool greater_equal<T>` 大于等于
- `template<class T> bool less<T>` 小于
- `template<class T>bool less_equal<T>`小于等于

```C
vector<int> v;
v.push_back(10);
v.push_back(20);
v.push_back(15);
//默认的sort是调用的less关系仿函数
sort(v.begin(),v.end(),Mycompare());
sort(v.begin(),v.end(),greater<int>());//两种方法一样
```

使用lambda函数代替cmp