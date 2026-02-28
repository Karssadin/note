---
tags: 
up:
  - "[[HTTPS]]"
down: 
relation:
---
- TLS（Transport Layer Security）和SSL（Secure Sockets Layer）是用于加密和保护网络通信的安全协议。
- HTTP（Hypertext Transfer Protocol）是一种用于在Web上进行数据通信的协议，它定义了客户端和服务器之间的请求和响应的格式和行为。HTTP本身是明文传输的，数据在传输过程中可以被篡改或窃取。
- HTTPS（HTTP Secure）是在HTTP和TLS/SSL之间的组合。它使用TLS/SSL协议对HTTP的通信进行加密和安全保护，以防止数据被篡改、窃取或伪造。使用HTTPS，通信双方之间的数据加密和身份验证可以提供更高的安全性。
- 当客户端通过HTTPS访问一个网站时，它会与服务器建立一个TLS/SSL连接。这个连接的建立经历了握手过程，包括协商加密算法、生成密钥、验证证书等步骤。一旦TLS/SSL连接建立成功，客户端和服务器之间的通信就会在加密的环境下进行，确保数据的机密性和完整性。