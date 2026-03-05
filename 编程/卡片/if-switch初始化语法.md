---
tags:
  - C++
up:
  - "[[C++新特性]]"
down:
relation:
---

C++17 允许在 `if` 和 `switch` 语句中添加初始化语句，缩小变量作用域。

## 语法

```cpp
if (init; condition) { ... }
switch (init; value) { ... }
```

## 典型用法

```cpp
// C++14：变量 it 泄漏到外层作用域
auto it = m.find(key);
if (it != m.end()) {
    use(it->second);
}
// it 仍然可见

// C++17：it 限定在 if 内
if (auto it = m.find(key); it != m.end()) {
    use(it->second);
}
// it 不可见，更安全
```

## 更多示例

```cpp
// 配合 lock_guard
if (std::lock_guard lg(mtx); shared_data.ready) {
    process(shared_data);
}

// 配合错误码
if (auto [iter, success] = map.insert({key, val}); !success) {
    std::cerr << "insert failed\n";
}

// switch 版本
switch (auto ch = get_char(); ch) {
    case 'a': handle_a(); break;
    case 'b': handle_b(); break;
    default: handle_other(ch);
}
```

## 优势

- 减少变量作用域污染
- 将相关的初始化和判断放在一起，提高可读性
- 配合结构化绑定使用效果更佳
