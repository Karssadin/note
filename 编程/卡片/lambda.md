---
tags:
  - C++
up:
  - "[[C++]]"
down:
relation:
  - "[[lambda：使用lambda代替cmp]]"
---
  

```C
[capture list] (parameter list) -> return type {function body }
// [捕获列表] (参数列表) -> 返回类型 {函数体 }
// 只有 [capture list] 捕获列表和 {function body } 函数体是必选的
auto lam =[]() -> int { cout << "Hello, World!"; return 88; };
auto ret = lam();
cout<<ret<<endl;    // 输出88
```

---

- 捕获列表：
- [] 不捕获任何变量,这种情况下lambda表达式内部不能访问外部的变量
- [&] 以引用方式捕获所有变量（保证lambda执行时变量存在）
- [=] 用值的方式捕获所有变量（创建时拷贝，修改对lambda内对象无影响)
- [=, &foo] 以引用捕获变量foo, 但其余变量都靠值捕获
- [&, foo] 以值捕获foo, 但其余变量都靠引用捕获
- [bar] 以值方式捕获bar; 不捕获其它变量
- **[this] 捕获所在类的this指针**

---

- 捕获指针可能会失效，如何避免?
	- 你只是捕获了一个指向某个内存地址的值，而不是这个地址中的实际数据。这意味着，如果该指针指向的对象生命周期结束，但lambda仍然存在并尝试访问该指针，就会导致未定义的行为，因为此时指针可能会指向无效的内存。
	- **使用****std::shared_ptr**：使用**std::shared_ptr**可以确保对象的生命周期至少与lambda相同。当lambda被销毁或超出范围时，**std::shared_ptr**的引用计数会减少，当计数减到0时，对象会被自动删除。
	- 确保lambda表达式不超过作用域

---

```C
int arr[] = {6, 4, 3, 2, 1, 5};
bool compare(int& a, int& b) {    //谓词函数
    return a > b;
}
std::sort(arr, arr + 6, compare);
//lambda形式
std::sort(arr, arr + 6, [](const int& a, const int& b){return a > b;});   //降序
```
