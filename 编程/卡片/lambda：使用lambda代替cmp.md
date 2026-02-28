---
tags: 
up: 
  - "[[函数对象-仿函数]]"
down: 
relation:
  - "[[lambda]]"
---
### 使用lambda函数代替cmp

使用lambda函数代替cmp从这两个方面，可以看出lambda表达式的优越性了。它的结构是这样的：[capture list](parameter list) ->return type {function body}其中，parameter list(参数列表)和function body(函数体)与上面的cmp函数没有什么差异。return type虽说必须尾置，但通常可以省略。而capture list(捕获列表)中可以填写作用域中的参数，使它在lambda表达式内部可见。这就解决了上面的第二个问题。比如说，我们要捕获b数组，就写成[&b]即可(&代表引用捕获)，也就是说捕获列表使lambda表达式可以使用不限量的外部参数，当然，如果不想指定捕获哪些，直接写[&]，就是全部引用捕获。同时，哪里用到，就写在哪里，还不用起名字(所以也叫匿名函数)，使它既直观又方便。