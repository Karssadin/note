---
tags: 
up: 
  - "[[常用函数、命名空间]]"
down: 
relation:
---
C++ setw() 函数用于设置字段的宽度，语法格式如下：

```Plain
setw(n)
```

n 表示宽度，用数字表示。

setw() 函数只对紧接着的输出产生作用。

**当后面紧跟着的输出字段长度小于 n 的时候，在该字段前面用空格补齐，当输出字段长度大于 n 时，全部整体输出。**

setw() 默认填充的内容为空格，可以 **setfill()** 配合使用设置其他字符填充。

```C
\#include <iostream>
\#include <iomanip>

using namespace std;

int main()
{
  cout << setfill('*') << setw(14) << "runoob" << endl;
  return 0;
}
```

以上代码输出结果为：

```Plain
********runoob
```