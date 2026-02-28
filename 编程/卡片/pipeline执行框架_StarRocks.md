---
tags: 
up:
  - "[[StarRocks]]"
down: 
relation:
---
### 1. **背景介绍**

- **Pipeline 调度**与传统的 **MPP 调度** 有显著差异。Pipeline 调度是单机多核调度，旨在：
    - 降低计算节点的任务调度代价。
    - 提升 CPU 利用率。
    - 充分利用多核计算能力，提高查询性能，自动设置并行度。

文章旨在帮助读者快速掌握如何在 **StarRocks** 中实现和使用 Pipeline 执行框架。

### 2. **基本概念**

- **MPP 调度基本概念**：
    
    - **物理执行计划 (ExecPlan)**：由物理算子构成的执行树，SQL 经处理后生成。
    - **计划碎片 (PlanFragment)**：物理执行计划的拆分部分，通过 **Exchange 算子** 实现上游与下游的数据传输。
    - **碎片实例 (Fragment Instance)**：分布式执行单位，每个 **PlanFragment** 可能有多个实例化的 **Fragment Instance**，实现数据并行计算。
    - **物理算子 (ExecNode)**：构成 **PlanFragment** 的基本单元，如 **OlapScanNode**、**HashJoinNode** 等。
- **Pipeline 调度基本概念**：
    
    - **Pipeline** 是由一组算子构成的执行链，其中：
        - **SourceOperator**：数据流的起点，读取数据。
        - **SinkOperator**：数据流的终点，输出数据。
    - 中间算子连接前后算子的输入和输出，形成数据处理链。

### 3. **Pipeline 执行过程**

- Pipeline 中的算子按顺序执行，数据从前到后传递。Pipeline 的执行过程中，算子会通过 `pull_chunk` 和 `push_chunk` 函数传递数据块（chunk）。
- **PipelineDriver** 是执行的基本单元，每个 Pipeline 可能实例化多个 **PipelineDriver**，并根据不同的并行度处理数据。
- **Pipeline 调度模型**：Pipeline 采用协程调度（用户态的 `yield` 语义），不同于操作系统的线程调度，避免了频繁的上下文切换，提高了 CPU 的利用率。
    - **PipelineDriver** 的状态有三种：**Ready**（准备就绪）、**Running**（运行中）和 **Blocked**（阻塞中）。

### 4. **阻塞操作异步化**

- 为了避免阻塞操作带来 CPU 利用率的下降，StarRocks 将阻塞操作异步化。常见的异步化操作包括：
    - **ScanOperator**：读取数据时的磁盘访问。
    - **ExchangeSinkOperator** 和 **ExchangeSourceOperator**：发送和接收数据时的网络操作。
    - **HashJoinProbeOperator**：等待 **HashJoinBuildOperator** 完成时的操作。

### 5. **BE 负责 Pipeline 调度**

- **BE 执行 PipelineDriver** 使用两种类型的线程：
    - **PipelineDriverExecutor**：不断从就绪队列获取 **PipelineDriver** 执行。
    - **PipelineDriverPoller**：负责检查阻塞队列中 **PipelineDriver** 的状态，解除阻塞并将其放回就绪队列。
- BE 通过 **就绪队列** 和 **阻塞队列** 调度 PipelineDriver，确保在高并发环境下高效利用计算资源。

### 6. **Pipeline 与 MPP 调度的差异**

- **MPP 调度** 依赖于分布式集群进行并行调度，而 **Pipeline 调度** 采用单机多核调度，注重任务的细粒度调度和协程调度的高效利用。

### 总结：

本文介绍了 **StarRocks Pipeline 执行框架** 的整体架构，着重讲解了如何通过 **Pipeline** 管理计算任务、如何将执行计划拆分为 Pipeline，以及如何处理异步阻塞操作等关键技术。Pipeline 调度优化了 CPU 利用率，提升了查询性能，是 StarRocks 在高并发和大数据量查询处理中的重要技术。