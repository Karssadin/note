---
tags: 
up:
  - "[[网络层：IP]]"
down:
  - "[[ICMP应用]]"
relation:
---
- ICMP，互联网控制报文协议，是TCP/IP协议簇的核心协议之一，主要用于IP层的错误报告和诊断工具。它允许主机和路由器报告错误和其他问题，并请求和提供有关网络状况的信息。
- **错误报告**: 如果路由器或主机不能处理IP数据报，ICMP会发送一个错误报文到源地址。
- **网络诊断**: 工具如ping和traceroute使用ICMP来测试网络连接和路径选择。
- 如主机 A 向主机 B 发送了数据包，由于某种原因，途中的路由器 2 未能发现主机 B 的存在，这时，路由器 2 就会向主机 A 发送一个 ICMP 目标不可达数据包，说明发往主机 B 的包未能成功。ICMP 的这种通知消息会使用 IP 进行发送 。因此，从路由器 2 返回的 ICMP 包会按照往常的路由控制先经过路由器 1 再转发给主机 A 。收到该 ICMP 包的主机 A 则分解 ICMP 的首部和数据域以后得知具体发生问题的原因。

---

- ICMP包头格式：
- **类型字段**: 表示报文的类型，如"目标不可达"或"时间超过"。
- **代码字段**: 提供与报文类型相关的额外信息。
- **校验和:** 用于检测报文在传输过程中的错误。
- **其他字段**: 取决于报文类型和代码，可以包含有关发送的原始数据报的信息或其他数据。
    
    [![](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/ping/5.jpg)](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/ping/5.jpg)
    

---

- **查询报文**: 用于网络诊断。例如：
    - 回显请求和回显应答 (ping使用)。
    - 时间戳请求和时间戳应答: 用于同步两台主机的时钟。
- **差错报文**: 用于报告问题。例如：
    
    - 目标不可达: 通常由路由器发送，告知发送者数据不能到达其目的地。
    - 源抑制: 由路由器发送，告知发送者减慢其数据发送速率。
    - 重定向: 当数据报可以更有效地通过另一个路由器传输时，原始路由器会向发送者发送此消息。
    
    [![](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/IP/41.jpg)](https://cdn.xiaolincoding.com/gh/xiaolincoder/ImageHost/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/IP/41.jpg)
    
