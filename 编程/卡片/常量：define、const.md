---
tags: 
up:
  - "[[变量和常量]]"
  - "[[关键字、修饰符、操作符、宏等]]"
down: 
relation:
  - "[[const和define的区别]]"
  - "[[常引用：const]]"
  - "[[static和const的区别]]"
  - "[[typedef和define的区别]]"
  - "[[constexpr和const的区别]]"
  - "[[常指针：const]]"
---
> **记录程序中不可更改的数据。**

> **常量可以是任何基本数据类型，可以分为整数，浮点数，字符，字符串和布尔值，在定义后无法修改**

---

- define预处理定义常量：`\#define identifier value`
- const定义常量：`const type variable = value;`
- 使用大写字母定义常量是一种很好的编程习惯

---

- 整型常量
    - 整型可以为十进制、八进制、十六进制，
    - 前缀：十六进制为0x、0X，八进制为0，十进制为空
    - 后缀：unsigned为U，long为L，可以为小写

```C
85         // decimal
0213       // octal
0x4b       // hexadecimal
30         // int
30u        // unsigned int
30l        // long
30ul       // unsigned longxxxxxxxxxx 85         // decimal0213       // octal0x4b       // hexadecimal30         // int30u        // unsigned int30l        // long30ul       // unsigned long//\#define 常量名 常量值#define day 7//const 数据类型 常量名 = 常量值;const int test = 10;
```

- 浮点型常量
    - 有一个整数部分，一个小数点，一个小数部分和一个指数部分。 可以以十进制形式或指数形式表示
    - 在使用小数形式表示时，必须包括小数点，指数或两者，并且在使用指数形式表示时，必须包括整数部分，小数部分或两者。 带符号的指数由e或E引入

```C
3.14159       // Legal
314159E-5L    // Legal
510E          // Illegal: incomplete exponent
210f          // Illegal: no decimal or exponent
.e55          // Illegal: missing integer or fraction
```

- bool型常量true、false，不去想非0即为真，这个有问题
- 字符串常量
    - 使用双引号括起来，可与分成多行

```C
"hello, dear"
"hello, \
dear"
"hello, " "d" "ear"
```