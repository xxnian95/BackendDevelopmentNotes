# CPU缓存

## CPU Cache结构
在CPU和主内存之间，通常有三级缓存：L1、L2、L3。其中，L1又分为L1i（instruction）和L1d（data）。CPU Cache的最小单位是Cache Line。其结构如下所示：
![][image-1]

整体的CPU和主内存之间交互的架构大致如下所示：
![][image-2]


## 缓存一致性

解决缓存一致性的方法主要有两种:
1. 总线加锁
2. 缓存一致性协议（如Intel的MESI协议）

总线加锁是一种比较悲观的解决方案，缓存一致性协议是目前首选的方法。其实现方式如下所示：
![缓存一致性协议解决数据不一致问题]()

## Java内存模型

Java内存模型是一个抽象的概念，与硬件上的CPU缓存结构完全不同。二者之间的关联如下所示：
![]()


[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191007202428.png
[image-2]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191007202526.png
