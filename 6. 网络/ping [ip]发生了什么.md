# ping [ip]发生了什么
#6. 网络/ip#
- - - -
ping基于**ICMP（Internet Control Message Protocal，Internet控制报文协议）**

从主机A上对主机B执行 ping [IP of B]
1. ping命令在A上构建一份**ICMP请求数据包**
2. ICMP协议将改请求数据包交给 [IP层协议](onenote:%23%E4%B8%89%E7%A7%8D%E7%BD%91%E7%BB%9C%E6%A8%A1%E5%9E%8B&section-id=%7BE7E0507C-CED5-7A4A-8EBD-1D2E7FE6F2FC%7D&page-id=%7BBEEBE56E-4EF1-074A-A4E4-F0A5C3C29618%7D&end&base-path=https://d.docs.live.net/0d8e5a53e747f609/%E6%96%87%E6%A1%A3/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91/6.%20%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%9F%BA%E7%A1%80/%E7%BD%91%E7%BB%9C.one) 
3. IP协议构建成一个**IP数据包**
4. IP协议获得A和B的MAC（ARP协议），一并交给**数据链路层**，组装成一个**数据帧**，用以太网传送出去
5. B收到这个数据帧时，检查MAC地址是不是自己，然后将数据中的IP数据包取出来，交给本机的IP层协议
6. IP层协议检查完后，取出ICMP数据包交给ICMP协议处理。处理完后，会构建一个ICMP应答数据包，回发给A
7. 一定的时间内，如果A收到了应答包，则说明与B之间网络可达
