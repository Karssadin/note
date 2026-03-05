---
tags:
  - 计算机网络
up:
  - "[[应用层：HTTP]]"
  - "[[HTTP1.0，1.1，2.0 的版本区别]]"
down:
relation:
  - "[[TCP UDP]]"
---

HTTP/3 是基于 **QUIC** 协议的 HTTP 版本，解决了 HTTP/2 中 TCP 层的队头阻塞问题。

## QUIC 协议

QUIC（Quick UDP Internet Connections）是 Google 开发、IETF 标准化的传输层协议，运行在 **UDP** 之上。

### 核心特性

| 特性 | 说明 |
|------|------|
| **基于 UDP** | 避免 TCP 的队头阻塞，但在应用层实现可靠性 |
| **连接迁移** | 使用 Connection ID（非 IP+端口），切换网络不断连（如 WiFi→4G） |
| **0-RTT 握手** | 首次连接 1-RTT，再次连接 0-RTT，大幅降低延迟 |
| **多路复用** | 每个流独立，一个流丢包不阻塞其他流 |
| **内置 TLS 1.3** | 加密集成在协议中，无需额外 TLS 握手往返 |

## HTTP 版本演进

| 版本 | 底层协议 | 关键改进 | 问题 |
|------|----------|----------|------|
| HTTP/1.0 | TCP | 基础请求/响应 | 每次请求新建连接 |
| HTTP/1.1 | TCP | Keep-Alive、管道化 | 队头阻塞（应用层） |
| HTTP/2 | TCP + TLS | 多路复用、头部压缩、Server Push | TCP 层队头阻塞 |
| HTTP/3 | QUIC（UDP） | 解决 TCP 队头阻塞、连接迁移 | 部署复杂，UDP 可能被防火墙拦截 |

## HTTP/2 vs HTTP/3 核心区别

**HTTP/2 的痛点**：虽然应用层解决了队头阻塞，但底层 TCP 一旦丢包，整个连接所有流都会停下来等待重传。

**HTTP/3 的解决**：QUIC 的每个流相互独立，一个流的丢包只影响该流，其他流正常传输。

## 部署情况

- 主流浏览器（Chrome、Firefox、Edge）和服务端（nginx 1.25+、Caddy）已支持
- 全球约 25%+ 的 Web 流量已使用 HTTP/3（主要是大厂 CDN）
