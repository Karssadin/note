---
tags:
  - 数据库
up:
  - "[[数据库总结]]"
  - "[[CMU15445]]"
down: 
relation: 
---
[[SQL基础语法]]
[[SQL数据类型与操作符]]
[[SQL高级特性]]
### **2.2 SQL 的高级特性**





- 结尾都要有分号
- SELECT FROM WHERE
- 函数：
    - AVG
    - MIN
    - MAX
    - SUM
    - COUNT
- GROUP BY
    - HAVING
- 字符串操作
    - CONCAT
- DATA/TIME
- 输出控制
    - ORDER BY
        - 默认ASC，可以设置DESC ORDER BY id DESC
    - LIMIT：输出N行
        - LIMIT 10 OFFSET 10，从第10行开始输出10行
- 子查询
    - ALL
    - ANY
    - IN
    - EXIST
- CTE：临时视图COMMON TABLE EXPRESSION