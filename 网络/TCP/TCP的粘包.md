# TCP的粘包

## 发送方产生粘包
采用TCP协议传输数据的客户端与服务器经常是保持一个长连接的状态（一次连接发一次数据不存在粘包），双方在连接不断开的情况下，可以一直传输数据；但当发送的数据包过于的小时，那么TCP协议默认的会启用*Nagle算法*，::将这些较小的数据包进行合并发送::（缓冲区数据发送是一个堆压的过程）；这个合并过程就是在发送缓冲区中进行的，也就是说数据发送出来它已经是粘包的状态了。
![][image-1]

## 接收方产生粘包
接收方采用TCP协议接收数据时的过程是这样的：数据到底接收方，从网络模型的下方传递至传输层，传输层的TCP协议处理是将其放置接收缓冲区，然后由应用层来主动获取（C语言用recv、read等函数）；这时会出现一个问题，就是我们在程序中调用的读取数据函数::不能及时的把缓冲区中的数据拿出来::，而下一个数据又到来并有一部分放入的缓冲区末尾，等我们读取数据时就是一个粘包。（放数据的速度 \> 应用层拿数据速度）

![][image-2]

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/37B8FADF-7C74-4D08-A9E8-F92FFEDD2F8E.36e4cf24441b4f78a0a6e843808f79df.jpg
[image-2]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/23D2AF33-D312-437D-81E6-945B7340B841.abe30cfaa32f487983b9ca1f10339bbe.jpg