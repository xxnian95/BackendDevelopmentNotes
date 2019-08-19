# Lock
#2. 并发# #待办事项/待完善的笔记
- - - -
[ReentrantLock实现原理深入探究 - 五月的仓颉 - 博客园](https://www.cnblogs.com/xrq730/p/4979021.html)
[Java并发-AQS及各种Lock锁的原理 - 不瘦十斤不换名字 - CSDN博客](https://blog.csdn.net/zhangdong2012/article/details/79983404)
- [ ] 待完善

- - - -
## Lock
Lock底层实现基于::AQS::实现，采用线程独占的方式，在硬件层面依赖特殊的::CPU指令（CAS）::。
简单来说，ReenTrantLock的实现是一种自旋锁，通过循环调用CAS操作来实现加锁。它的性能比较好也是因为::避免了使线程进入内核态的阻塞状态::。想尽办法避免线程进入内核的阻塞状态是我们去分析和理解锁设计的关键钥匙。

## AQS
AQS是AbustactQueuedSynchronizer的简称，它是一个Java提高的底层同步工具类，用一个int类型的变量表示同步状态，并提供了一系列的CAS操作来管理这个同步状态。AQS的主要作用是为Java中的并发同步组件提供统一的底层支持，例如ReentrantLock，CountdowLatch就是基于AQS实现的，用法是通过继承AQS实现其模版方法，然后将子类作为同步组件的内部类。

