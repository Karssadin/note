---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
---
- [[#常用函数|常用函数]]
- [[#构造函数|构造函数]]
- [[#赋值操作|赋值操作]]
- [[#字符串拼接|字符串拼接]]
- [[#字符串查找和替换|字符串查找和替换]]
- [[#字符串比较|字符串比较]]
- [[#字符存取|字符存取]]
- [[#字符串插入和删除|字符串插入和删除]]
- [[#子串获取-截取子串|子串获取-截取子串]]
- [[#返回字符数组对应的头指针|返回字符数组对应的头指针]]

----
- C++风格的字符串，string本质是一个类
- 与char*的区别：char *是个指针。string是个类，其中封装了char* 管理这个字符串
- 不用担心赋值越界，取值越界等，内部进行负责

### 常用函数

- `size()`
- `length()`
- `empty()`
- `begin()、end()`

### 构造函数

- `string()`:构造一个空的字符串
- `string(const char*s)`:使用字符串s进行初始化
- `string(const string&str)`:使用string对象初始化另一个string对象
- `string(int n,char c)`:使用n个字符c进行初始化

### 赋值操作

- `string& operator=(const char* s);` char*类型字符串赋值给当前的字符串
- `string& operator=(const string &s);` 把字符串s赋给当前的字符串
- `string& operator=(char c);` 字符赋值给当前的字符串
- `string& assign(const char *s);` 把字符串s赋给当前的字符串
- `string& assign(const char*s, int n);` 把字符串s的前n个字符赋给当前的字符串
- `string& assign(const string &s);` 把字符串s赋给当前字符串
- `string& assign(int n,char c);` 用n个字符c赋给当前字符串等号方法比较常用，assign记住就好

### 字符串拼接

- `string& operator+=Tconst char* str);` 重载+=操作符
- `string& operator+=(const char c);` 重载+=操作符
- `string& operator+=(const string& str);` 重载+=操作符
- `string& append(const char *s);` 把字符串s连接到当前字符串结尾
- `string& append(const char *s, int n);` 把字符串s的前n个字符连接到当前字符串结尾
- `string& append(const string &s);` 同operator+=(const string& str)
- `string& append(const string &s, int pos，int n);` 字符串s中从pos开始的n个字符连接到字符串结尾

### 字符串查找和替换

- 查找：查找指定字符串是否存在
    - `int find(const string& str, int pos =0) const;` 查找str的一次出现位置,从pos开始查找，如果没有就返回-1
    - `int find(const char* s, int pos = 0) const;` 查找s第一次出现位置,从pos开始查找，如果没有就返回-1
    - `int find(const char* s, int pos, int n) const;` 从pos位置查找s的前n个字符第一次位置
    - `int find(const char c, int pos = e) const;` 查找字符c第一次出现位置
    - `int rfind(const string& str, int pos = npos)const;` 查找str最后一次位置,从pos开始查找，从右向左查找
    - `int rfind(const char* s, int pos = npos)const;` 查找s最后一次出现位置,从pos开始查找，从右向左查找
    - `int rfind(const char* s, int pos, int n)const;` 从pos查找s的前n个字符最后一次位置
    - `int rfind(const char c, int pos = e) const;` 查找字符c最后一次出现位置
- 替换：在指定的位置替换字符串
    - `string& replace(int pos, int n, const string& str);` 替换从pos开始n个字符为字符串str，将pos之后，n个字符替换为str，不管str是多长
    - `string& replace(int pos, int n,const char* s);` 替换从pos开始的n个字符为字符串s

### 字符串比较

- 字符串之间的比较，按照ASCII码进行比较
- 等于返回0，小于返回-1，大于返回1
- `int compare(const string &s) const`;与字符串s比较
- `int compare(const char*s) const`

### 字符存取

- `char& operator[](int n);`通过[]方式取字符
- `char& at(int n);` 通过at方法获取字符
- 也可以通过该方法进行修改对应位置的值

### 字符串插入和删除

- `string& insert(int pos,const char* s );` 插入字符串
- `string& insert(int pos,const string& str);` 插入字符串
- `string& insert(int pos, int n，char c);` 在指定位置插入n个字符c
- `string& erase(int pos, int n = npos);` 删除从Pos开始的n个字符

### 子串获取-截取子串

- `string substr(int pos = 0，int n = npos) const;`返回由pos开始的n个字符组成的字符串

```C
	string email="test@email.cn";
	string usrName=email.substr(0,email.find('@'));
```

### 返回字符数组对应的头指针

- `c_str()`
- `printf("%s",s.sc_str());`