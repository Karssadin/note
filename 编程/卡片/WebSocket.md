---
tags:
  - 计算机网络
up:
  - "[[应用层：HTTP]]"
down:
relation:
  - "[[HTTP协议]]"
  - "[[HTTP长连接和短连接概念和应用场景]]"
---

WebSocket 是一种在单个 TCP 连接上进行**全双工通信**的协议，由 HTML5 提出，解决 HTTP 轮询的低效问题。

## HTTP vs WebSocket

| 维度 | HTTP | WebSocket |
|------|------|-----------|
| 通信方式 | 请求-响应（半双工） | 全双工 |
| 连接复用 | 短连接/Keep-Alive | 持久连接 |
| 服务器推送 | 不支持（需轮询） | 支持 |
| 协议头开销 | 每次请求都有完整头 | 建立后数据帧极小 |
| 适用场景 | 普通请求-响应 | 实时通信 |

## 握手过程（基于 HTTP 升级）

```
客户端 → 服务端（HTTP Upgrade 请求）：
GET /chat HTTP/1.1
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

服务端 → 客户端（101 Switching Protocols）：
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

握手完成后，连接升级为 WebSocket 协议，不再使用 HTTP。

## 数据帧格式

WebSocket 数据以帧（Frame）传输，支持：
- `Text`：UTF-8 文本
- `Binary`：二进制数据
- `Ping/Pong`：保活
- `Close`：关闭连接

## 应用场景

- **实时聊天**：IM、客服系统
- **游戏**：多人在线实时同步
- **协作编辑**：多人同时编辑文档
- **金融**：股票行情实时推送
- **监控大盘**：服务器指标实时展示

## 与 SSE（Server-Sent Events）的比较

| 维度 | WebSocket | SSE |
|------|-----------|-----|
| 方向 | 双向 | 单向（服务端推送） |
| 协议 | ws:// / wss:// | 基于 HTTP |
| 浏览器支持 | 广泛 | 广泛（IE 除外） |
| 适用 | 需要客户端发送数据 | 只需服务端推送 |
