---
tags: 
up:
  - "[[容器、适配器、工具]]"
down: 
relation:
---
- `先进后出`，只有一个出口，只有栈顶的元素才能被外界使用，栈`不允许有遍历行为`

### 常用接口

- 构造函数
    - `stack<T> stk` `stack`采用模板类实现,`stack`对象的默认构造形式
    - `stack(const stack &stk);` 拷贝构造函数
- 赋值操作:
    - `stack& operator=(const stack &stk);` 重载等号操作符
- 数据存取:
    - `push(elem);` 向栈顶添加元素
    - `pop();` 从栈顶移除第一个元素
    - `top();`返回栈顶元素
- 大小操作:
    - `empty();`判断堆栈是否为空
    - `size();`返回栈的大小
- 没有clear函数