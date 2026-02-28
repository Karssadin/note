# GBase8a qtree 引擎性能调优指南 V1.0

对应版本: 9.5.3.050_patch.15

## 概念

**qtree 与 8a 的关系可以简单描述为: qtree 替换了 8a 原有的查询优化器（集群层）和执行器（单机层），其它部分共用**

在8a中，处理一条 select 查询语句的基本流程如下，开启qtree后相同的步骤省略

|步骤|不开启 QTree|开启 QTree|
|---|---|---|
|1|将SQL编译为抽象语法树||
|2|生成逻辑查询计划|对抽象语法树进行改写，适配 CBO 优化器|
|3|使用 gcplanner 优化器进行优化，输出执行计划并转换为 SQL|使用 CBO 优化器进行优化，输出 json 格式的分布式执行计划|
|4|将 SQL 下发到单机|将 json 格式的执行计划下发到单机|
|5|单机对 SQL 进行解析并执行|qtree 执行器按执行计划执行查询|
|6|express 执行器按需从 express 存储引擎读取数据|qtree 执行器按需从 express 存储引擎读取数据|


下面以一条简单查询初步介绍 qtree 中的执行计划

```
explain select sum(l_orderkey) from lineitem group by (l_partkey);
```
1. 每个主分片按照 `l_partkey` 进行分组，并计算 `sum(l_orderkey)`
2. 由于 `lineitem` 表并不是按 `l_partkey` 进行哈希分布的，所以每个分片上按 `l_partkey` 分组聚合的结果是不完整的，需要将中间结果按 `l_partkey` 进行重分布，然后进行第二阶段的聚合
3. 每个分片上二阶段聚合的结果就是最终的结果，最后需要将它们收集在一起并返回给客户端

在4主分片的集群上，它的执行计划如下（同时给出 express 执行计划作为参照）
```bash
# qtree 的执行计划
->{GatherMotion:InputSegments : 0,1,2,3 OutputSegments: -1 }  # 3
    ->{Result:{ProjectList Num:1 } }
        ->{Aggreate Node:{ProjectList Num:2 }{ Group BY:L_PARTKEY, } }  # 2
            ->{Redistribute Motion InputSegments : 0,1,2,3 OutputSegments: 0,1,2,3}  # 2
                ->{Result:{ProjectList Num:2 } }
                    ->{Aggreate Node:{ProjectList Num:2 }{ Group BY:L_PARTKEY, } }  # 1
                        ->{TableScan : Table name : tpch.lineitem{ProjectList Num:2 }

# express 的执行计划
+----+---------------------+-----------+----------------------+--------------------+
| ID | MOTION              | OPERATION | TABLE                | CONDITION          |
+----+---------------------+-----------+----------------------+--------------------+
| 01 | [RESULT]            |  Step     | <00>                 |                    |
|    |                     |  GROUP    |                      | GROUP BY l_partkey |
| 00 | [REDIST(l_partkey)] |  Table    | lineitem[l_orderkey] |                    |
|    |                     |  GROUP    |                      | GROUP BY l_partkey |
+----+---------------------+-----------+----------------------+--------------------+
```

按照 `Motion` 算子可将整个分布式执行计划拆分为几个部分，每个部分被称作一个 `Slice`，每个 Slice 根据其用途可能产生一个或多个实例，从而在一个或多个分片上执行
```bash
# Slice -1，汇总步骤，只在第一个分片上执行，Slice 只有一个实例
->{GatherMotion <'RECV'>:InputSegments : 0,1,2,3 OutputSegments: -1 }  # 3

# Slice 1，第二阶段聚合，在所有分片上执行，Slice 有 4 个实例
->{GatherMotion <'SEND'>:InputSegments : 0,1,2,3 OutputSegments: -1 }  # 3
    ->{Result:{ProjectList Num:1 } }
        ->{Aggreate Node:{ProjectList Num:2 }{ Group BY:L_PARTKEY, } }  # 2
            ->{Redistribute Motion <'RECV'> InputSegments : 0,1,2,3 OutputSegments: 0,1,2,3}  # 2

# Slice 2，第一阶段聚合，在所有分片上执行，Slice 有 4 个实例
            ->{Redistribute Motion <'SEND'> InputSegments : 0,1,2,3 OutputSegments: 0,1,2,3}  # 2
                ->{Result:{ProjectList Num:2 } }
                    ->{Aggreate Node:{ProjectList Num:2 }{ Group BY:L_PARTKEY, } }  # 1
                        ->{TableScan : Table name : tpch.lineitem{ProjectList Num:2 }
```
`Slice` 的概念将分布式执行计划转换为多个单机执行计划，`Slice实例` 是 qtree 执行器执行查询的基本单位，所有 `Slice实例` 会一并下发到单机进行执行，Motion操作越多的查询，slice 数量越多，执行和调度过程也就越复杂。qlog中的profile信息，是以 `Slice实例` 为单位输出的


## 基本操作

### 启用 qtree
开启后，查询语句将通过 qtree 执行
```
set _gbase_enable_qtree = 1;
```

### 启用 qtree DML
`_gbase_enable_qtree` 关闭时，此参数的值无效

开启后，DML 语句将通过 qtree 执行
```
set _gbase_enable_dml_qtree = 1;
```

### 查看 qlog
qlog 是目前 qtree 输出日志的主要途径，需要优先关注。qlog中包含的 `runtime profile` 是分析 qtree 性能瓶颈的主要手段之一

qlog 存储在共享内存中，首先使用 `qlog -l` 查看当前节点上的 qlog 文件列表
```
Qlog: find 2 log files, as bellow: 
        qlog.gc_gbase_9988
        qlog.gn_gbase_172.156.0.2_9988
```
上述输出中，qlog.gc_gbase_9988 表示 gcluster 服务产生的 qlog，
qlog.gn_gbase_172.156.0.2_9988 表示 gnode 服务产生的 qlog

如果采用多实例部署，gnode 的 qlog 会有多个，以文件名中的 ip 作为区分

以上述 gnode 节点的 qlog 为例，使用以下命令查看 qlog 中的所有内容
```
qlog qlog.gn_gbase_172.156.0.2_9988
```

不查看历史内容，仅追踪最新的 qlog 记录并实时输出到终端
```
qlog -cf qlog.gn_gbase_172.156.0.2_9988
```

以日志级别进行过滤，不查看 warning 以下级别的日志，可选的日志级别有 `debug`, `trace`, `info`, `warn`, `error`
```
qlog qlog.gn_gbase_172.156.0.2_9988 -F warn
```

## 性能指标

### runtime profile (输出在 qlog 中)
Runtime profile 以 Slice 实例为单位进行输出，建议以第一个节点的 qlog 中的 profile 信息入手进行分析，因为它包含 Slice -1 的 profile 信息

```yaml
Slice:(Active: 320.870ms, non-child: 100.00%)
   - Slice ID: -1
   - Segment ID: 0
   - Query ID: 6029598
   - Operator DOP: 30
```

查询性能基本分析流程
1. 查看 slice -1 的 `Active` 时间，应略快于客户端显示的执行时间。若相差较大，则首先查看 gcluster qlog 确认是否是生成计划耗费了较多时间（查看此类日志：Optimize finished in: 0.019 seconds），若不是则考虑是返回结果集耗时较高，在 gccli 命令中附加 `-q` flag 观察查询结束时间是否缩短
2. 查看其它 slice 的 `Active` 时间，时间极短的 slice 可以暂时忽略，定位到耗时较高的一个或多个 slice
3. 观察这些 slice 的调度器指标，按以下注释分析调度是否存在问题
```yaml
SessionJobScheduler:
    - info: Limit(192),EnableReorder(false),ReorderPolicy(1),ReorderThreshold(96)
    - JobsPerSecond: 11.08 K/sec
    - RunningTime: 89.242ms
    - ScheduleCount: 989 (989)
    - ScheduleWaittingTime: 6.895ms  # 若此项较高，说明算子 DOP 过高或线程池线程数量过少
    - YieldCount: 0 (0)              # 若下面两项较高，则说明当前 slice 中存在某些有明显性能瓶颈的算子，它们处理数据的速度远低于下游算子输出数据的速度。查找当前 slice 中 YieldByOutputFull 较高的算子即可对应上。例如 B 算子的 yield 较高，而 A 算子的 Input 是 B 算子，则性能瓶颈在 A 算子
    - YieldRate(%): 0 (0)
```
4. 通过上一步的 yield 定位到瓶颈算子，或逐个排查 slice 中算子的关键指标，确定性能瓶颈点。以下列出一些关键算子的关键性能指标
```yaml
  MotionSend(7):
    # 该 Motion 算子的工作模式，本例中为哈希重分布
    - MotionType: Redistribute
  
      # 本例中的集群有 4个节点，以下指标显示重分布到各节点的数据行数。若行数较多且存在严重不均，可能存在数据倾斜问题
    - Server0SendRows: 2 (2)
    - Server1SendRows: 2 (2)
    - Server2SendRows: 2 (2)
    - Server3SendRows: 3 (3)

  Result(6):
    ResultMetrics:
       # Actions 中可能包含表达式计算或过滤操作
       - ActionsTime: 241.000ns
         - __MAX_OF_ActionsTime: 3.869us
         - __MIN_OF_ActionsTime: 0.000ns

  Aggregator(5):
     # 聚合之前的表达式计算耗时，分别是线程平均耗时和总耗时
     - PrepareActionsAverageTime: 9.345us
     - PrepareActionsTimeAllThreads: 149.520us

     # 聚合计算耗时
     - RealAggregateAverageTime: 1.223ms
     - RealAggregateCalculateTimeAllThreads: 12.322ms
     - RealAggregateTimeAllThreads: 19.572ms
     - RealAggregateTimeSlowestThread: 1ms

     # 本地汇总聚合结果耗时
     - RealMergeTimeSingleThread: 14.026ms

     # 输入和输出行数，能反应聚合度
     - TotalOutputRows: 70 (70)
     - TotalSrcRows: 13.16K (13161)

  Join(4):
     - JoinType: Inner  # join 类型
     - HybridJoin: 1    # 是否启用了 hybrid join

     # 左右表输入算子，右表是期望的小表，用于建哈希
     - LeftInput: Aggregator(2)
     - RightInput: Scan(3)
     
     - BuildHashTable: 14.017ms      # 右表建哈希以及 RightInput 向下递归所有耗时的总和
     - BuildRowsRecv: 14.32K (14322) # 右表行数
     - JoinedRows: 13.16K (13161)    # join 输出的行数
     - SpilledBlocksJoin: 0.000ns    # 如果该时间不是 0，则说明发生了落盘
     - SplitBlock: 2.883ms           # 将右表输入拆分给各个工作线程的耗时

     # 生成 runtime filter 的时间，通常很短，若较高则需要权衡 RF 开启的收益
     - UpdateBloomFilterTime: 698.275us
     - UpdateMinMaxFilterTime: 507.362us
    Slot Profiles:
       - OutputBuildColumn: 140.538us  # 输出结果集中右表部分的耗时
         - __MAX_OF_OutputBuildColumn: 290.834us
         - __MIN_OF_OutputBuildColumn: 34.784us
       - OutputProbeColumn: 16.771us   # 输出结果集中左表部分的耗时
         - __MAX_OF_OutputProbeColumn: 41.523us
         - __MIN_OF_OutputProbeColumn: 3.961us
       - ProbeMatch: 310.583us  # 匹配哈希表的耗时
         - __MAX_OF_ProbeMatch: 614.304us
         - __MIN_OF_ProbeMatch: 38.960us
       - ProbeRowsRecv: 19.07K (19069)  # 左表行数
         - __MAX_OF_ProbeRowsRecv: 2.40K (2403)
         - __MIN_OF_ProbeRowsRecv: 147 (147)
       - ProbeTotalTime: 38.386us  # 单个线程 probe 的总耗时，实际耗时取决于最慢的一个
         - __MAX_OF_ProbeTotalTime: 77.809us
         - __MIN_OF_ProbeTotalTime: 4.568us

  ParallelScan(3):
    InputStreamMetrics:
       # 两种 runtime filter 的数量
       - BloomFilterCount: 0
       - MinMaxFilterCount: 0

       - BSIFilteredDCs: 0 (0)                 # 智能索引直接过滤掉的无效 DC 个数
       - ActionOutputRows: 14.32K (14322)      # 表达式过滤后剩余的有效行数
       - BloomFilterOutputRows: 14.32K (14322) # bloom filter 过滤后剩余的有效行数（先表达式过滤，后 bloom filter 过滤）
       - FillBlockTime: 11.530ms               # 从 DC 读取数据的耗时，包括存储引擎层的耗时
       - TotalOutputRows: 14.32K (14322)       # 经过所有过滤后，实际输出的行数
       - TotalScannedRows: 374.41K (374406)    # 需要扫描的原始行数，不包括智能索引过滤掉的
```


### perf top

perf top 主要用于观测底层性能热点，同时也是检查性能异常的有效手段。以下列举几种常见的性能异常现象以及应对思路

#### 1. 内核函数调用占比最高，如 queued_spin_lock 等
1. 确认 [TCMALLOC_AGGRESSIVE_DECOMMIT](#TCMALLOC_AGGRESSIVE_DECOMMIT) 是否设置为 0 
2. 尝试降低 DOP 和 scan DOP

#### 2. MemoryTracker 相关函数占比最高
关闭 [QTREE_ENABLE_MEM_TRACKER](#QTREE_ENABLE_MEM_TRACKER)

### nmon
主要用来观察 CPU 占用率、磁盘 IO 情况和网络 IO 情况

## 表结构优化
优化原则
1. 能 not null 的列全部定义为 not null
2. 能使用更短数据类型容纳的字段，一律使用更短的数据类型
3. 分布列，排序和分区表根据实际情况按常规思路创建即可，qtree无特殊需求

备注
1. 目前哈希索引和全文索引对 qtree 没有任何作用

## gbase 参数调优
参数名中不包含 `qtree` 的参数，是新引擎和老引擎通用的参数

可以通过以下命令获取相关参数的值
```bash
gccli -uroot -e "show variables like '%qtree%'; show variables like '%direct_send%'; show variables like '%distribution_number%'; show variables like '%serial_exec%';";
gncli -uroot -e "show variables like '%qtree%'; show variables like '%caching%'; show variables like '%gbase_cache_type%';";
```

### 常用参数

#### gbase_qtree_parallel_degree
- 默认值: 0
- 作用: 简称为 DOP，控制除 scan 外的算子并行度，设为0时默认为环境变量参数 [QTREE_THREAD_COUNT](#QTREE_THREAD_COUNT) 的 1/2
- 调优建议: 多实例部署时建议根据实例数手动计算，设为 `CPU 核数 / 实例数`

#### gbase_qtree_scan_parallel_degree
- 默认值: 0
- 作用: 简称为 Scan DOP，控制 scan 算子的并行度，设为0时默认与 [gbase_qtree_parallel_degree](#gbase_qtree_parallel_degree
) 相同
- 调优建议: 一般情况下设为 0 即可，根据实际情况可调整为小于 `gbase_qtree_parallel_degree` 的值

#### gcluster_qtree_runtime_rf_max_ndv
- 默认值: 20000000
- 作用: runtime filter 的 NDV 最大值
- 调优建议: 根据当前版本的经验为当主分片个数大于 8 个时，可以尝试将该值调整为 2000000

#### gbase_qtree_buffer_agg
- 默认值: 2147483648 (2G)
- 作用: 在聚合算子中每个并行任务（数量由DOP决定）使用的内存上限，超过时当前并行任务处理的数据会进行落盘
- 调优建议: 当运行环境的内存足够大时，调高该值（设为超过节点物理内存，等同于无限大）避免落盘以发挥最大性能

#### gbase_qtree_buffer_mergeagg
- 默认值: 2147483648 (2G)
- 作用: 两阶段聚合的第二阶段使用的内存总上限，超过时进行落盘
- 调优建议: 同 `gbase_qtree_buffer_agg`

#### gbase_qtree_buffer_cte
- 默认值: 2147483648 (2G)
- 作用: CTE 算子可使用的内存总上限，超过时进行落盘
- 调优建议: 同 `gbase_qtree_buffer_agg`

#### gbase_qtree_buffer_join
- 默认值: 2147483648 (2G)
- 作用: Join 算子可使用的内存总上限，超过时进行落盘。[gcluster_qtree_enable_hybrid_join](#gcluster_qtree_enable_hybrid_join) 关闭时该上限无效
- 调优建议: 同 `gbase_qtree_buffer_agg`

#### gbase_qtree_buffer_sort
- 默认值: 2147483648 (2G)
- 作用: 在排序算子中每个并行任务（数量由DOP决定，但至多8个）使用的内存上限，超过时当前并行任务处理的数据会进行落盘
- 调优建议: 同 `gbase_qtree_buffer_agg`

#### _gbase_caching_level
- 默认值: 0
- 作用: 是否开启 DC cache
- 调优建议: DC cache 对于重复扫描小批量数据（查询读取的数据可以部分或完全容纳在DC堆中）的场景有较大提升

#### gbase_sql_trace_level
- 默认值: 0
- 作用: 日志级别
- 调优建议: 对于 qtree，该参数控制 qlog 中显示的日志级别。需要查看 profile 信息时必须设置为 3 或以上

#### _t_gcluster_support_cte
- 默认值: OFF
- 作用: 是否开启 CTE 语法支持
- 调优建议: 建议开启，否则不支持 TPCDS 等包含 CTE 的场景

#### _gcluster_optimizer_push_condition
- 默认值: ON
- 作用: 是否开启条件下推优化
- 调优建议: 对于 qtree，该参数必须关闭，否则会导致 TPCDS 中部分查询结果集错误

### 特殊优化参数

#### gcluster_serial_exec_query
- 默认值: 0
- 作用: 控制集群是否将并发查询串行化，开启时无论有多少并发查询输入，集群只会逐条进行处理
- 调优建议: 由于目前 qtree 在查询并发控制方面的能力较差，若测试场景要求并发并导致 qtree 性能异常下降，则考虑开启此参数

#### gcluster_parallel_distribution_number
- 默认值: 0
- 作用: 该参数不为0时启用分步下发功能，控制集群至多同时向每个gnode实例上的多少个主分片下发执行计划
- 调优建议: 当业务场景为大数据量 insert select DML，且查询部分不存在数据重分布时，考虑使用多主分片部署并开启此参数。该优化的思路是将大数据量 DML 拆解成互相独立的小 DML 并分批执行，使内存使用峰值成倍减少并避免中间结果落盘。假设某业务场景数据量超过 1T 时发生内存不足，则可以部署两主分片并将此参数设为1，此时相当于串行执行了两个数据量为 500G 的业务 SQL，不会发生内存不足问题

#### gbase_cache_type
- 默认值: 0 
- 作用: 修改 DC 堆缓存淘汰策略，0为LIRS，1为LRU
- 调优建议: 当 [_gbase_caching_level](#_gbase_caching_level) 设为 1 时，通过观察 `select * from performance_schema.cache_usage_info;` 中的缓存命中率决定是否需要调整淘汰策略。

#### gcluster_qtree_optimizer_plan_num
- 默认值: 0
- 作用: 是否读取外部执行计划
- 调优建议: 特殊优化手段，当优化器输出的执行计划不合理时，可以将手写的合理计划放置到 `$GCLUSTER_BASE/userdata/gcluster/minidumps` 目录下并命名为 `N.mdp`，之后在执行SQL的session中设置`set gcluster_qtree_optimizer_plan_num = N`，即可通过给定的执行计划执行 SQL

#### gcluster_qtree_enable_motion
- 默认值: 1
- 作用: 特殊优化手段，设为0时会强制去除执行计划中的所有motion算子
- 调优建议: 一般无需关注

#### gbase_insert_select_direct_send
- 默认值: OFF
- 作用: DML 参数，是否启用 insert select 本地写入
- 调优建议: 若测试场景不需要备份分片，且使用 qtree 执行 insert select 时，建议开启此参数

#### gbase_qtree_col_insert
- 默认值: OFF
- 作用: qtree DML 参数，是否开启按列写入 temptable
- 调优建议: 启用 qtree DML 时建议开启

### 其它参数

#### gcluster_qtree_auto_exchange_join_lr
- 默认值: 1
- 作用: 在优化器输出执行计划后，是否根据估算行数对左右表顺序不合理的join进行左右置换操作
- 调优建议: 一般无需关注

#### gcluster_qtree_enable_hybrid_join
- 默认值: 1
- 作用: 是否开启 hybrid join
- 调优建议: 一般无需关注，hybrid join 适合主分片数量较少的部署方式，有必要采用多主分片部署时再考虑关闭

#### gcluster_qtree_enable_local_motion
- 默认值: 1
- 作用: 是否开启 motion 算子的本地直通
- 调优建议: 一般无需关注

#### gcluster_qtree_global_runtime_filter
- 默认值: 1
- 作用: 是否开启全局 runtime filter
- 调优建议: 一般无需关注，进一步调优需要分析profile中的性能统计。主分片数量大于 16 个时可以考虑关闭，需要实际测试后确定

#### gcluster_qtree_rf_adaptive_local_bloom_filter
- 默认值: 1
- 作用: 是否开启本地bloom filter自适应
- 调优建议: 一般无需关注

#### gcluster_qtree_rf_ndv_downscale_factor
- 默认值: 1
- 作用: 将 runtime filter 的 NDV 大小强制缩小 N 倍
- 调优建议: 当 bloom filter 大小评估不合理时，强制对其进行缩小

#### gcluster_qtree_rf_ndv_max_pct_of_estimate_rows
- 默认值: 200
- 作用: 使用估算行数对 runtime filter 的 NDV 进行上限限制，默认为 200%
- 调优建议: 当 bloom filter 大小评估不合理时，强制对其进行限制

#### gcluster_qtree_runtime_bloom_filter
- 默认值: 1
- 作用: 启用 runtime bloom filter，允许 join 算子根据右表生成 bloom filter 对左表的 scan 进行过滤
- 调优建议: 一般无需关注

#### gcluster_qtree_runtime_minmax_filter
- 默认值: 1
- 作用: 启用 runtime minamx filter，允许 join 算子统计右表最大最小值并在左表的 scan 使用 BSI 进行过滤
- 调优建议: 一般无需关注

#### gcluster_qtree_runtime_rf_min_ndv
- 默认值: 0
- 作用: runtime filter 的 NDV 最小值
- 调优建议: 一般无需关注

#### gbase_qtree_buffer_compress
- 默认值: OFF
- 作用: 算子落盘文件是否进行压缩
- 调优建议: 一般无需关注

#### gbase_qtree_dynamic_dop
- 默认值: OFF
- 作用: 从环境变量 QTREE_INSTANCE_NUM （需手动设置）读取当前节点的实例数并自动计算并行度（自动覆盖 gbase_qtree_parallel_degree 和 gbase_qtree_scan_parallel_degree）
- 调优建议: 非多实例、多主分片部署的情况下无需关注

#### gbase_qtree_enable_express_ref_column
- 默认值: ON
- 作用: 启用 qtree 引用列功能，减少从 express 的内存拷贝
- 调优建议: 一般无需关注

#### gbase_qtree_express_ref_column_shrink_threshold
- 默认值: 20
- 作用: 百分比，qtree 引用列功能自适应重整的阈值，当 block 经过滤后剩余的行数低于该比例时，转换为拷贝列
- 调优建议: 一般无需关注

#### gbase_qtree_part_select_threshold
- 默认值: 18446744073709551615
- 作用: 执行层进行运行时分区筛选时，命中的行数超过该值后不再进行分区筛选
- 调优建议: 一般无需关注

#### gbase_qtree_sort_cache_block_size
- 默认值: 2048000
- 作用: 排序算子每轮排序前累积的数据行数
- 调优建议: 一般无需关注

#### gcluster_rebalancing_concurrent_count
- 默认值: 5
- 作用: 后台 rebalance 线程数
- 调优建议: 一般无需关注，如果环境执行过 rebalance，会有部分线程在后台循环执行状态查询语句，导致qlog中输出大量无效信息，此种情况建议将该值设为 0

### 环境变量
环境变量需要添加到所有节点的 gbase_profile 中，修改环境变量后需要重启服务 (如无特殊说明，都是重启gnode服务)

**如果直接在终端使用 `gcluster_services` 命令重启，则需要确保环境变量在当前终端已生效，若使用 c3/pssh 等工具则无需关注，因为它们会创建新的终端并自动加载 gbase_profile 中的环境变量**

#### TCMALLOC_AGGRESSIVE_DECOMMIT
- 默认值: 1
- 作用: tcmalloc 是否将 cache 中的内存立刻归还给操作系统
- 调优建议: 对于 qtree 来说，该参数几乎必须调整为 0 ，否则将严重影响性能

#### ROWS_PER_BLOCK
- 默认值: 4096
- 作用: qtree 算子中按批处理数据时，每批数据的行数
- 调优建议: 建议调整为 8192

#### QTREE_QUEUE_LENGTH
- 默认值: 1024
- 作用: qtree 算子间流式传输数据时使用的队列长度上限
- 调优建议: 一般无需关注

#### QTREE_SCTL_LENGTH
- 默认值: 4096
- 作用: qtree 进行网络传输时使用的本地缓冲队列长度上限
- 调优建议: 若查询执行过程中发生卡住现象，且 `perf top` 可以观察到 `StreamWithCtrl` 相关调用，考虑将该值设为 10240 或更高

#### QTREE_ENABLE_MEM_TRACKER
- 默认值: 1
- 作用: 启用内存追踪器，该功能的主要作用是配合 `QTREE_MEMORY_POOL` 来限制 qtree 的内存使用上限
- 调优建议: 一般无需关注，当追求极致性能或`perf top`中显示 `MemoryTracker` 采样比例较高时，可以关闭。在ARM平台下对性能产生影响的可能性较高，建议关闭

#### QTREE_ENABLE_CONTEXT_MEM_TRACKER
- 默认值: 1
- 作用: 启用 Slice 实例级别的内存追踪器，当 `QTREE_ENABLE_MEM_TRACKER` 开启时才会生效，用于统计每个 Slice 实例的内存使用峰值并记录在 profile 中，辅助性能分析
- 调优建议: 同 `QTREE_ENABLE_MEM_TRACKER`

#### QTREE_THREAD_COUNT
- 默认值: 0
- 作用: qtree 进程级线程池的线程工作线程数，设为 0 时自动取 1.5 倍核数
- 调优建议: 多实例部署时建议手动调节为 `核数 / 实例数 * 1.5`

#### QTREE_MEMORY_POOL
- 默认值: 0
- 作用: qtree 进程级内存池的容量上限，配合 `QTREE_ENABLE_MEM_TRACKER` 达到限制内存用量的目的，设为 0 时自动取节点物理内存的 20%
- 调优建议: 若 8a 集群可以独占节点的资源，建议设置为与物理内存一致(单位为 byte)。如果 `QTREE_ENABLE_MEM_TRACKER` 关闭，则该值无效，不需要关注

#### ENABLE_COROUTINE
- 默认值: 1
- 作用: 启用基于协程的算子任务调度框架
- 调优建议: 一般无需关注

#### ENABLE_SMART_SCAN
- 默认值: 1
- 作用: 数据扫描时启用智能索引过滤
- 调优建议: 一般无需关注

#### MEMLOG_RECORD_LENGTH
- 默认值: 10240
- 作用: qlog 中每条日志的最大长度，单位为 byte
- 调优建议: 若部分 profile 信息较长，显示不完整，则需要调大该值

#### MEMLOG_RECORD_NUM
- 默认值: 16384
- 作用: qlog 最大能容纳的日志条数
- 调优建议: 一般无需关注，若共享内存较小，则调小该值

#### QTREE_INSTANCE_NUM
- 默认值: 1
- 作用: 手动设置实例数，为程序提供多实例部署信息（目前集群和单机层服务均对多实例部署无感知）
- 调优建议: 一般无需关注，在特殊场景下配合 [gbase_qtree_dynamic_dop](#gbase_qtree_dynamic_dop) 使用

## 常见问题

### 内存不足

内存不足分为两种情况
#### 1. QTREE 内存池容量不足

现象为客户端报错
```
attempt to allocate chunk of xxx, maximum: xxx
```
此时应考虑通过环境变量 `QTREE_MEMORY_POOL` 扩大内存池容量，或直接关闭 `QTREE_ENABLE_MEM_TRACKER`，不对 qtree 内存使用做限制（一旦系统内存不足会直接 OOM）

#### 2. 系统内存不足
现象为节点离线，`dmesg` 可看到 OOM 日志

此时优先考虑降低 [gbase_qtree_parallel_degree](#gbase_qtree_parallel_degree) 以及 [gbase_qtree_scan_parallel_degree](#gbase_qtree_scan_parallel_degree)。若仍然存在内存不足情况，则尝试下调算子buffer的尺寸 ([gbase_qtree_buffer_agg](gbase_qtree_buffer_agg) 以及其它几个算子buffer)。下调buffer尺寸的目的是使内存使用过多的算子进行中间结果落盘，可以通过 nmon 的 disk监控信息、`$GBASE_BASE/tmpdata/cache_gbase/`下生成临时文件以及profile信息确认算子是否进行落盘


### 查询卡死
1. 排查是否有节点宕机，后被 gcmonit 重新拉起。若有则实质上是宕机问题，查看宕机堆栈
2. 查看是否有 [QTREE_SCTL_LENGTH](#QTREE_SCTL_LENGTH) 中所述现象
3. 查看所有节点的 qlog，以 error 级别进行过滤，确认是否有错误信息
3. 在所有 gnode 上 `show processlist`，查看哪个节点上存在未结束的任务。在存在任务的节点上收集去重堆栈


