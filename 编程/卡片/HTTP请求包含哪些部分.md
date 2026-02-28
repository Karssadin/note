---
tags: 
up:
  - "[[HTTP协议]]"
down: 
relation:
  - "[[HTTP常用方法]]"
  - "[[GET请求中URL编码的意义]]"
  - "[[Cookie和Session的区别？]]"
  - "[[在浏览器中输入 URL 地址到显示主页的过程？]]"
---
- **请求行（Request Line）**：包含了**请求方法、请求的URL和HTTP协议的版本**。例如：
- **请求头部（Request Headers）**：包含了关于请求的附加信息，以键值对的形式表示。常见的请求头包括：
    - Host：指定要访问的服务器主机名或IP地址。
    - User-Agent：发送请求的用户代理的信息，通常是浏览器的名称和版本。
    - Content-Type：指定请求体的媒体类型，如application/json、multipart/form-data等。
    - Accept：指定客户端可接受的响应内容类型。
    - Authorization：包含了身份验证凭证，用于访问需要身份验证的资源。
    - Cookie：包含了在之前的请求或响应中设置的Cookie信息。
    - 空行（Blank Line）：用于分隔请求头部和请求体，通常是一个空行。
- **请求体（Request Body）**：可选的，用于包含请求的数据，例如在POST请求中传递表单数据或JSON数据。