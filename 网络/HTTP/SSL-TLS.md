# SSL/TLS

* **SSL（Secure Socket Layer）**是基于HTTPS下的一个协议加密层。
* 在SSL3.0的基础上诞生了**TLS1.0（Tranport Layer Security，安全传输层协议）**。

1. **SSL(Secure Sockets Layer安全套接层)**，及其继任者**安全传输层（Transport Layer Security，TLS）**是为网络通信提供安全及数据完整性的一种安全协议。TLS与SSL在传输层对网络连接进行加密。
2. _SSL协议位于TCP/IP协议与各种应用层协议之间，为数据通讯提供安全支持。_ SSL协议可分为两层： SSL记录协议（SSL Record Protocol）：它建立在可靠的传输协议（如TCP）之上，为高层协议提供数据封装、压缩、加密等基本功能的支持。 SSL握手协议（SSL Handshake Protocol）：它建立在SSL记录协议之上，用于在实际的数据传输开始前，通讯双方进行身份认证、协商加密算法、交换加密密钥等。
3. 安全传输层协议（TLS）用于在两个通信应用程序之间提供保密性和数据完整性。该协议由两层组成： TLS 记录协议（TLS Record）和 TLS握手协议（TLS Handshake）。
4. TLS 的**最大优势**就在于：TLS 是独立于应用协议。高层协议可以透明地分布在 TLS 协议上面。然而，TLS 标准并没有规定应用程序如何在 TLS 上增加安全性；它把如何启动 TLS握手协议以及如何解释交换的认证证书的决定权留给协议的设计者和实施者来判断。