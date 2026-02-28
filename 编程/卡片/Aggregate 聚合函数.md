---
tags:
  - 数据库
up:
  - "[[CMU15445]]"
down: 
relation:
---
- 聚合函数将一组元组作为输入，然后生成单个标量值作为输出。只能在`SELECT`中使用
- `SQL-92`标准中定义了：
	- `ACG(col)`：返回`col`列的平均值。
		- `select AVG(gpa) from student;`
	- `MIN(col)`：返回`col`列的最小值。
		- `select MIN(gpa) from student;`
	- `MAX(col)`：返回`col`列的最大值。
		- `select MAX(gpa) from student;`
	- `SUM(col)`：返回`col`列所有值的和。
		- `select sum(gpa) from student;`
	- `COUNT(col)`：返回`col`列的个数。
		- `select count(sid) from enrolled;`
	- `sum`和`count`可以添加`distinct`去重。
		- `select count(distinct sid) from enrolled;`

---
- `Group by`对给出的一个或多个属性进行分组，属性上取值相同的被分为一组，进行聚合。可以利用`having`设置分组的限定条件
	- `select`和`having`中，只能有被聚集的列和出现在`group by`中的列
	- 如果不使用`Group by`的话，将所有结果视为一组
```sql
select 
	avg(s.gpa) as avg_gpa, 
	e.cid from enrolled as e, 
	student as s 
where 
	e.sid = s.sid 
group by 
	e.cid 
having 
	avg(s.gpa) > 3.9;
```