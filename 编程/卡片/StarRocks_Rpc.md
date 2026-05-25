---
tags:
  - 数据库
  - StarRocks
up:
  - "[[StarRocks_Operator]]"
down:
  - "[[编程/画板/StarRocks_Rpc|StarRocks_Rpc画板]]"
relation:
  - "[[pipeline执行框架_StarRocks]]"
  - "[[qtree]]"
---

# StarRocks_Rpc

StarRocks RPC 笔记用于承接画板中的 Exchange / RPC 链路，避免只有 Excalidraw 图而没有文字入口。

## 关注点

1. ExchangeSink / ExchangeSource 在分布式执行中的数据传输职责。
2. Pipeline 执行框架中阻塞网络发送的异步化处理。
3. BRPC 通道、发送窗口和 backpressure 的关系。
4. qtree 迁移或借鉴 StarRocks RPC 模型时，需要拆分协议、通道管理、算子接口和调度唤醒。

## 相关资料

1. [[pipeline执行框架_StarRocks]]
2. [[StarRocks_Operator]]
3. [[编程/画板/StarRocks_Rpc|StarRocks_Rpc画板]]
