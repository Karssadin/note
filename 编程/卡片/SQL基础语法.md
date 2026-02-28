---
tags:
  - 数据库
up:
  - "[[SQL语言]]"
down: 
relation:
---
- **DDL（Data Definition Language）**：用于定义数据库结构和对象。
    - 创建表：`CREATE TABLE`
    - 修改表：`ALTER TABLE`
    - 删除表：`DROP TABLE`
- **DML（Data Manipulation Language）**：用于操作表中数据。
    - 插入数据：`INSERT INTO`
    - 更新数据：`UPDATE`
    - 删除数据：`DELETE`
- **DQL（Data Query Language）**：用于查询数据。
    - 查询数据：`SELECT`

```sql

```
## select 
> 用于从表中提取数据
- 指定列名（* 表示全部）：
```sql
SELECT id, name 
FROM employees;
```
- 使用 `WHERE` 筛选条件：  
```sql
SELECT * 
FROM employees
WHERE salary > 5000;
```
- 使用排序：  
```sql
SELECT name, salary 
FROM employees 
ORDER BY salary DESC;
```
- 使用limit限制返回行数：
```sql
SELECT * 
FROM employees 
LIMIT 10 OFFSET 5;
```
## insert
- 插入单条数据：
```sql
INSERT INTO employees (name, salary, department_id) 
VALUES ('John Doe', 5000, 3);
```
- 插入多条数据：

```sql
INSERT INTO employees (name, salary, department_id) 
VALUES      ('Alice', 6000, 1),    
('Bob', 7000, 2);
```
- 使用 `INSERT SELECT` 从另一表（可以是本身）插入数据：   
```sql
INSERT INTO employees_backup (name, salary, department_id) 
	SELECT name, salary, department_id 
	FROM employees 
	WHERE department_id = 2;
```
## update
- 更新指定列的值：
```sql
UPDATE employees SET salary = salary + 1000 WHERE department_id = 2;
```
- 更新多列：
```sql
UPDATE employees SET salary = 5000, department_id = 1 WHERE id = 101;
```
## delete
- 删除符合条件的行：
```sql
DELETE FROM employees WHERE department_id = 3;
```
- 删除表中所有行（慎用）：
```sql
DELETE FROM employees;
```
## create

- 创建表：
```sql
CREATE TABLE employees (    
id INT PRIMARY KEY,     
name VARCHAR(50) NOT NULL,     
salary DECIMAL(10, 2),     
department_id INT );
```
    
- 创建数据库：
```sql
CREATE DATABASE company_db;
```   
- 创建视图：
```sql
CREATE VIEW high_salary_employees AS 
SELECT name, salary 
FROM employees 
WHERE salary > 7000;
```
## alter

- 添加列：
```sql
ALTER TABLE employees ADD COLUMN hire_date DATE;
```
- 修改列的数据类型：
```sql
ALTER TABLE employees MODIFY COLUMN salary FLOAT;
```
- 删除列：
```sql
ALTER TABLE employees DROP COLUMN hire_date;
```
- 重命名表：
```sql
ALTER TABLE employees RENAME TO staff;
```

   ## drop
- 删除表：
```sql
DROP TABLE employees;
```    
- 删除数据库：
```sql
DROP DATABASE company_db;
```   
- 删除视图：
```sql
DROP VIEW high_salary_employees;
```