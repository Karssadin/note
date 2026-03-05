---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
  - "[[编程/卡片/字符串]]"
---

C++17 引入 `std::string_view`，提供对字符串的零拷贝只读视图。

## 解决的问题

```cpp
// C++14：传递字符串常量会构造临时 string
void print(const std::string& s);
print("hello");  // 构造临时 std::string，分配堆内存

// C++17：零拷贝
void print(std::string_view sv);
print("hello");           // 不分配内存
print(some_string);       // 不拷贝
print(some_string.substr(0, 5));  // string 的 substr 会拷贝
```

## 基本操作

```cpp
std::string_view sv = "hello world";

sv.substr(0, 5);     // "hello"，返回 string_view，不拷贝
sv.find("world");    // 6
sv.remove_prefix(6); // sv 变为 "world"
sv.remove_suffix(1); // sv 变为 "worl"
sv.size();           // 长度
sv.empty();          // 是否为空
sv.data();           // 底层指针
```

## 注意事项

- `string_view` 不拥有数据，必须确保底层字符串的生命周期
- 不保证以 `\0` 结尾，不能直接传给 C 函数
- 不要返回局部 `string` 的 `string_view`

```cpp
// 危险！
std::string_view bad() {
    std::string s = "hello";
    return s;  // s 销毁后 string_view 悬垂
}
```
