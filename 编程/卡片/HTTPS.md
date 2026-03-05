---
tags:
  - 计算机网络
up:
  - "[[应用层：HTTP]]"
down:
  - "[[HTTPS的连接过程]]"
  - "[[HTTPS 的优缺点]]"
  - "[[TLS SSL,HTTP,HTTPS的关系]]"
relation:
  - "[[加密与认证]]"
  - "[[HTTP协议]]"
  - "[[网络安全基础]]"
---

## HTTPS 是什么

**HTTPS**（HyperText Transfer Protocol Secure）= HTTP + TLS/SSL

在 HTTP 之上添加了 **TLS（Transport Layer Security）** 层，提供：
1. **加密**（Encryption）：防止数据被窃听
2. **完整性**（Integrity）：防止数据被篡改（MAC）
3. **身份认证**（Authentication）：通过数字证书验证服务器身份

---

## HTTP vs HTTPS 对比

| 维度 | HTTP | HTTPS |
|------|------|-------|
| 传输安全 | 明文传输，可被窃听 | 加密传输 |
| 数据完整性 | 无校验，可被篡改 | MAC 校验 |
| 身份认证 | 无法验证服务器身份 | CA 证书验证 |
| 默认端口 | 80 | 443 |
| 性能开销 | 无额外开销 | TLS 握手开销（约1个RTT，TLS 1.3可0-RTT）|
| CA 证书 | 不需要 | 需要（Let's Encrypt 可免费申请）|
| SEO | 排名较低 | 谷歌等优先收录 |

---

## 加密机制

HTTPS 综合使用**非对称加密**和**对称加密**：

```
握手阶段（非对称加密）：
  客户端 → 服务器公钥加密 → 协商出会话密钥（Master Secret）

数据传输阶段（对称加密）：
  使用协商好的会话密钥（如 AES-128-GCM）加密所有通信数据
```

**为什么不全程用非对称加密**？非对称加密（RSA/ECDH）计算代价高，仅用于密钥协商；对称加密（AES）速度快，用于大量数据传输。

---

## 证书与信任链

```
Root CA（受信根证书）
  └── 中间 CA
        └── 服务器证书（域名 example.com）
```

- 客户端操作系统/浏览器内置**信任的根 CA 列表**
- 服务器在握手时发送自己的证书
- 客户端验证证书链：是否由受信 CA 签发、是否过期、域名是否匹配

---

## TLS 1.2 vs TLS 1.3

| 特性 | TLS 1.2 | TLS 1.3 |
|------|---------|---------|
| 握手往返次数 | 2-RTT | 1-RTT |
| 0-RTT 恢复 | 不支持 | 支持（Session Ticket）|
| 密钥交换 | RSA / ECDHE | 仅 ECDHE（强制前向保密）|
| 加密套件 | 多种（含弱算法）| 精简为5种安全套件 |
| 前向保密 | 可选 | 强制 |

详见 [[HTTPS的连接过程]]、[[TLS SSL,HTTP,HTTPS的关系]]
