---
tags: 
up:
  - "[[TCP三次握手]]"
down: 
relation:
---
- （ISN）是固定的吗?
    - 三次握手的一个重要功能是客户端和服务端交换ISN(Initial Sequence Number), 以便让对方知道接下来接收数据的时候如何按序列号组装数据。
    - 如果ISN是固定的，攻击者很容易猜出后续的确认号，因此 ISN 是动态生成的。