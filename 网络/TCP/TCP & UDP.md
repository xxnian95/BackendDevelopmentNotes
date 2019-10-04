# TCP & UDP

- 用户数据报协议 UDP（User Datagram Protocol）是无连接的，尽最大可能交付，没有拥塞控制，*面向报文*（对于应用程序传下来的报文不合并也不拆分，只是添加 UDP 首部），支持一对一、一对多、多对一和多对多的交互通信。 传输控制协议
- TCP（Transmission Control Protocol）是面向连接的，提供可靠交付，有流量控制，拥塞控制，提供全双工通信，面向字节流（把应用层传下来的报文看成字节流，把字节流组织成大小不等的数据 块），每一条TCP连接只能是点对点的（一对一）。
	 
![][image-1]

## 对应的协议
- TCP
	1. FTP：文件传输协议
	2. Talnet：远程登录端口
	3. SMTP：简单邮件传输协议
	4. POP3：接收邮件
	5. HTTP：超文本传送协议
- UDP
	1. ::DNS::：域名解析服务
	2. SNMP：简单网络管理协议
	3. TFTP：简单文件传输协议。

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191005020347.png