# TCP的快重传、快恢复
#6. 网络/TCP#
- - - -
* 接收方接收到失序的报文段后立马发出重复确认。
* 发送方连续收到三个重复确认后，重新发送丢失的报文段，不必等待设置的重传计时器到期。

# 快恢复
1. 发送方连续收到三个重复确认时，执行“乘法减小”算法，把ssthresh门限减半，但并不执行慢开始。
2. 将cwnd（当前拥塞窗口）设置为ssthresh的大小，执行拥塞避免算法。
 [https://blog.csdn.net/wdscq1234/article/details/52529994](https://blog.csdn.net/wdscq1234/article/details/52529994) 
