---
tags:
  - go
  - 工程化
up:
  - "[[Go工程化]]"
down:
relation:
  - "[[GDB]]"
  - "[[Linux]]"
---

# Go测试与性能分析

Go 内置测试框架，测试文件以 `_test.go` 结尾，常用 `testing` 包编写单元测试、benchmark 和示例测试。

## 测试

```bash
go test ./...
go test -run TestName ./pkg
go test -race ./...
```

## Benchmark

```bash
go test -bench=. -benchmem ./...
```

## pprof

1. CPU profile：定位 CPU 热点。
2. heap profile：定位内存分配和保留对象。
3. goroutine profile：排查 goroutine 泄露和阻塞。
4. block / mutex profile：分析阻塞和锁竞争。

## 常见关注点

1. 表驱动测试。
2. race detector 检查数据竞争。
3. benchmark 需要避免编译器优化干扰。
