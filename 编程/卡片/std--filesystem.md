---
tags:
  - STL
up:
  - "[[C++新特性]]"
down:
relation:
---

C++17 引入 `std::filesystem`，提供跨平台的文件系统操作库。

## 头文件与命名空间

```cpp
#include <filesystem>
namespace fs = std::filesystem;
```

## 路径操作

```cpp
fs::path p = "/home/user/file.txt";

p.filename();      // "file.txt"
p.stem();          // "file"
p.extension();     // ".txt"
p.parent_path();   // "/home/user"
p.root_path();     // "/"

fs::path joined = p.parent_path() / "other.txt";
```

## 文件操作

```cpp
fs::exists(p);                   // 是否存在
fs::is_regular_file(p);          // 是否普通文件
fs::is_directory(p);             // 是否目录
fs::file_size(p);                // 文件大小（字节）

fs::create_directory("new_dir");
fs::create_directories("a/b/c"); // 递归创建
fs::copy("src.txt", "dst.txt");
fs::remove("file.txt");
fs::remove_all("dir");           // 递归删除
fs::rename("old", "new");
```

## 目录遍历

```cpp
// 非递归遍历
for (const auto& entry : fs::directory_iterator("/path")) {
    std::cout << entry.path() << "\n";
}

// 递归遍历
for (const auto& entry : fs::recursive_directory_iterator("/path")) {
    if (entry.is_regular_file())
        std::cout << entry.path() << " " << entry.file_size() << "\n";
}
```
