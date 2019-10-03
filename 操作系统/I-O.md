# I/O

## BIO: Blocking IO
- 同步并阻塞
- 一个连接一个线程
- 适用于::链接数目比较小::且固定的架构
是一个比较传统的通信方式，模式简单，使用方便。但 _并发处理能力低，通信耗时，依赖网速_ 。

## NIO: Non-blocking IO
- 是Java SE 1.4版以后，针对网络传输效能优化的新功能。是一种*非阻塞同步*的通信模式。
- 同步非阻塞
- 一个请求一个线程
- 原来的 I/O 以流的方式处理数据，而 NIO 以块的方式处理数据。
- 适用于::连接数目较多并且比较短（轻操作）::的架构，比如 _聊天服务器_
 [https://blog.csdn.net/wsh_8818/article/details/2953137]()(https://blog.csdn.net/wsh_8818/article/details/2953137) 

## AIO: Asynchronous-IO
> Asynchronous：异步的
异步非阻塞
一个有效请求一个线程
适用于::链接数目多并且连接比较长（重操作）::的架构，比如相册服务器

1. 阻塞IO：socket 的阻塞模式意味着*必须要做完IO*操作（包括错误）才会返回。
2. 非阻塞IO：非阻塞模式下无论操作是否完成都会立刻返回，需要通过其他方式来判断具体操作是否成功。

阻塞/非阻塞、同步/异步的深入理解
 [https://blog.csdn.net/lengxiao1993/article/details/78154467]()(https://blog.csdn.net/lengxiao1993/article/details/78154467)  

- *阻塞式发送（blocking send）：*发送方进程会被一直阻塞， 直到消息被接受方进程收到。
- *非阻塞式发送（nonblocking send）：* 发送方进程调用 send() 后， 立即就可以其他操作。
- *阻塞式接收（blocking receive）：*接收方调用 receive() 后一直阻塞， 直到消息到达可用。
- *非阻塞式接受（nonblocking receive）：* 接收方调用 receive() 函数后， 要么得到一个有效的结果， 要么得到一个空值， 即不会被阻塞。

## I/O
IO，常协作I / O，是Input / Output的简称，即输入/输出。通常指数据在 _内部存储器（内存）和外部存储器（硬盘、优盘等）_ 或其他周边设备之间的输入和输出。

