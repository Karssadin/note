---
tags:
  - 数据库
up:
  - "[[qtree_优化器]]"
down:
relation:
  - "[[qtree_统计信息优化调研]]"
---
## 现状
- 执行流程
	1. 创建系统表（如需）
	2. 表级sql：
		1. select count(*) from <table_name>;
		2. 插入数据
		3. 如果count() < 1,return，不进行列级查询
	3. 列级sql（并行与串行:
		1. NULL
			1. select count(*) from <table_name> where <col_name> is null;
		2. width
			1. select cast(avg(length<col_name>) as signed) from <table_name> where <col_name> is not null;
		3. 在需要计算dixtinct、mcv、直方图的情况下
			1. create table tmpTable nocpoies 
			2. select <col_name>,count(*)  from <table_name> group by <col_name>
			3. 满足条件情况下，后续的计算可以直接使用该表中的数据进行计算
				1. tableName = tmpTable
		4. distinct
			1.  select count(*) from (select <col_name> from <table_name> group by <col_name>) tb);
				1. 如果不同值数据为0，设置统计值为0，禁用MCV和直方图计算
				2. 如果采样数据中所有值都不同，设置特殊标记，禁用MCV计算
				3. 之后再计算MCV和直方图
		5. MCV
		6. 直方图


