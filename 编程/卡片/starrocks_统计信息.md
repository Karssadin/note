---
tags:
up:
  - "[[StarRocks]]"
down:
relation:
  - "[[StarRocks_统计信息收集 代码逻辑图.canvas|StarRocks统计信息收集 代码逻辑图]]"
  - "[[qtree_统计信息优化调研]]"
---
- [[#收集|收集]]

- 整理网上资料：[StarRocks 统计信息和 Cost 估算 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/582214743)
- 阅读源码：
---
- Cost 估算的基础是统计信息估算，统计信息估算的基础是统计信息收集。 StarRocks 目前支持表级别和列级别的统计信息，支持自动收集和手动收集两种方式，无论自动还是手动，都支持全量收集和抽样收集两种方式。
---
- starrocks中统计信息（Statistics Class) 描述了表中数据的详细信息，包含表的行数和每一列的数据分布：**最大/最小值，不同值的个数**（NDV）**，NULL** **值个数和列的平均大小**（Average Row Size）。 因此整体流程可以大致分为两部分，分别是**统计信息的收集**和**统计信息的读取计算**。
- 该文件只整理统计信息收集相关内容
- 统计信息的计算主要在优化器部分进行
---
下图描述了统计信息的收集和读取计算的整体流程：classM <|.. classN

[[StarRocks统计信息收集流程图.png]]

- 统计信息的收集包括手动和定期任务这两种触发方式，对应了图中的两种 Statement ：**CreateAnlyzeJobStmt**（创建 Analyze 定期任务）和 **AnalyzStmt**（手动执行 Analyze 命令）。两种方式都会创建一个 AnalyzeJob，由它负责具体的统计信息的收集，收集的类型包含全量（**FULL**）和抽样（**SAMPLE**）两种。
- 而收集到的统计信息会存储在 BE 的` _statistics_.table_statistic_v1` 表中。其中记录了不同 table 每一列的统计信息

## 收集
- CreateAnalyzeStmt 通过 StatisticAutoCollector 周期性地调度 AnalyzeJob，周期间隔由_statistic_collect_interval_sec_ 决定。
- AnalyzeStmt 会立即触发一次 AnalyzeJob 的执行，且只执行一次。  
	- 还需注意的是，StatisticAutoCollector 不仅包含用户通过 CreateAnalyzeStmt 创建的 AnalyzeJob，还包括为所有表创建一个**默认**的抽样任务，用于定期更新统计信息（
    - 如下图所示，我们细化了上一小节整体[[StarRocks统计信息收集流程图.png]]中的 **AnalyzeJob** 任务。AnalyzeJob 在执行具体的收集任务时，首先会创建多个 **TableCollect Job**。而每个 **TableCollect Job** 又会负责收集对应 Table 的统计信息，收集过程中还会使用 **StatisticsExecutor** 来负责实际的统计信息的写入。如下图所示：
![](https://pic2.zhimg.com/80/v2-6df699d7d82e50427522b138eb545889_720w.webp)

- 这里要注意的是，StatisticsExecutor 可以简单理解为生成对应表的 **Insert into select** 语句并执行，下面是生成 SQL 语句时的模板。执行该语句时，会将对应的 Table 的列统计信息记录在 **table_statistic_v1** 表中：

```java
private static final String INSERT_STATISTIC_TEMPLATE = "INSERT INTO " + Constants.StatisticsTableName;

private static final String INSERT_SELECT_FULL_TEMPLATE =
        "SELECT $tableId, '$columnName', $dbId, '$tableName', '$dbName', COUNT(1), "
                + "$dataSize, $countDistinctFunction, $countNullFunction, $maxFunction, $minFunction, NOW() "
                + "FROM $tableName";
```

## 源码分析
### StatisticBuild
- 通过**动态选择和生成 SQL** 来处理统计信息查询、插入或删除操作。它主要通过 **Velocity 模板引擎** 来完成这一过程。简单来说，程序会根据你输入的参数（比如表的 ID、列名、分区等）自动选择合适的 SQL 模板，并动态填充这些参数生成最终的 SQL 语句
