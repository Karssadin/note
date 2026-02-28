---
tags: 
up: 
  - "[[控制结构]]"
down: 
relation:
  - "[[循环语句：while、for]]"
---
- **if语句可以嵌套**
    - if(条件){条件满足执行的语句}
    - if(条件){条件满足执行的语句}else{条件不满足执行的语句}
    - if(条件){条件1满足执行的语句}else if(条件2){条件2满足执行的语句}……else{上面多个条件都不满足执行的语句}

---

- 三目运算符
    - if和else的简写三目运算符返回的是表达式？**表达式1 ?表达式2:表达式3**
    - if和else的简写三目运算符返回的是表达式？**表达式1 ?表达式2:表达式3**

```C
//b为a、c中的大者
int b = (a > c) ? a : c;
```

---

- switch

```C
switch (choice){
		case 1: cout << "\a\n";
			break;
		case 2: report();
			break;
		case 3: cout << "The boss was in all day .\n";
			break;
		case 4:comfort();
			break;
		default: cout << "没有这个选项.\n";
			break;
}
/*
switch(表达式){
	case 结果1：执行语句;break;
	case 结果2：执行语句;break;
	……
	default:执行语句;break;
}
*/
```

- 根据表达式的结果判断执行哪个语句
    - 只能使用整型或者浮点型，其他类型不可以，与if可以使用区间不同，这里只能是值，不可以是区间
    - 对于boolean value，会有警告，因为bool值使用if-else即可