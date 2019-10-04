## 服务器出现大量的`TIME_WAIT`
在::高并发短连接::的TCP服务器上，当服务器处理完请求后立刻主动正常关闭连接。
为什么我们要关注这个高并发短连接呢？有两个方面需要注意：
1. ::高并发::可以让服务器在短时间范围内同时占用大量端口，而端口有个065535的范围，并不是很多，刨除系统和其他服务要用的，剩下的就更少了。
	2. 在这个场景中，::短连接::表示“业务处理+传输数据的时间 远远小于 TIMEWAIT超时的时间”的连接。
### 解决方案
打开系统的TIME_WAIT重用和快速回收。
编辑内核文件/etc/sysctl.conf，加入以下内容：
	net.ipv4.tcp_syncookies = 1 表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；
	net.ipv4.tcp_tw_reuse = 1 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
	net.ipv4.tcp_tw_recycle = 1 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭。
	net.ipv4.tcp_fin_timeout 修改系默认的 TIMEOUT 时间
然后执行 `/sbin/sysctl -p` 让参数生效。
	/etc/sysctl.conf是一个允许改变正在运行中的Linux系统的接口，它包含一些TCP/IP堆栈和虚拟内存系统的高级选项，修改内核参数永久生效。
