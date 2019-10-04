# `ping [ip]`发生了什么

ping基于**ICMP（Internet Control Message Protocal，Internet控制报文协议）**

从主机A上对主机B执行 `ping IP of B`
1. ping命令在A上构建一份**ICMP请求数据包**
2. ICMP协议将改请求数据包交给 [IP层协议]()
3. IP协议构建成一个**IP数据包**
4. IP协议获得A和B的MAC（ARP协议），一并交给**数据链路层**，组装成一个**数据帧**，用以太网传送出去
5. B收到这个数据帧时，检查MAC地址是不是自己，然后将数据中的IP数据包取出来，交给本机的IP层协议
6. IP层协议检查完后，取出ICMP数据包交给ICMP协议处理。处理完后，会构建一个ICMP应答数据包，回发给A
7. 一定的时间内，如果A收到了应答包，则说明与B之间网络可达

