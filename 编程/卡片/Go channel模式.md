---
tags:
  - go
  - 并发
up:
  - "[[Go并发编程]]"
down:
relation:
  - "[[channel]]"
  - "[[select]]"
  - "[[context]]"
  - "[[goroutine]]"
---

# Go channel模式

channel 不只是队列，也常用于表达任务流、生命周期和背压。

## 常见模式

1. pipeline：多个阶段通过 channel 串联。
2. fan-out：多个 worker 从同一个输入 channel 取任务。
3. fan-in：多个输出 channel 合并为一个输出。
4. worker pool：固定数量 goroutine 消费任务，限制并发度。
5. done channel：广播退出信号，实际项目中更常用 `context`。

## 注意

1. 发送方负责关闭 channel。
2. 下游退出时要通知上游停止生产，避免 goroutine 泄露。
3. 有缓冲 channel 可以削峰，但不能替代明确的并发控制。
4. 多发送方关闭同一个 channel 容易 panic，应由协调者关闭。
