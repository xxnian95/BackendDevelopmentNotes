# synchronized

## 概述
- `synchronized`关键字提供了一种锁的机制，能够保证**共享变量的互斥访问**，从而防止数据不一致问题的出现。
- `synchronized`关键字包括`monitor enter`和`monitor exit`两个JVM指令，它能够保证在任何时候任何线程执行到`monitor enter`成功之前都必须从主内存中获取数据，而不是从缓存中，在`moniter exit`运行成功之后，共享变量被更新后的值必须刷入主内存。![]()
- `synchronized`的指令严格遵守Java [happens-before][1]规则，一个`monitor exit`指令之前必定要有一个`monitor enter`。
Synchronized keyword enable a simple strategy for preventing thread interference and memory consistency errors: if an object is visible to more than one thread, all reads or writes to that object’s variables are done through synchronized methods. --Official Manual

## 作用
- 修饰*代码块*，即同步语句块，其作用的范围是大括号括起来的代码，作用的对象是调用这个代码块的对象。
- `this monitor`：修饰*普通方法*，即同步方法，其作用的范围是整个方法，作用的对象是调用这个方法的对象。
- `class monitor`：修饰*静态方法*，其作用的范围是整个静态方法，作用的对象是这个类的所有对象。

JVM 是通过进入、退出**对象监视器(Monitor)**来实现对方法、同步块的同步的，而对象监视器的本质依赖于底层操作系统的**互斥锁(Mutex Lock)**实现。
具体实现是在编译之后在同步方法调用前加入一个`monitorenter`指令，在退出方法和异常处插入`monitorexit`的指令。
对于没有获取到锁的线程将会阻塞到方法入口处，直到获取锁的线程`monitorexit`之后才能尝试继续获取锁。

## 特点
![]()

## 原理
在synchronized修饰方法时是添加ACC`_SYNCHRONIZED`标识，该标识指明了该方法是一个同步方法，JVM通过该`ACC_SYNCHRONIZED`访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。
synchronized属于**重量级锁**，效率低下，因为监视器锁（monitor）是依赖于底层操作系统的**Mutex Lock**来实现的，而_操作系统实现线程之间的切换时需要从用户态转换到核心态_，这个状态之间的转换需要相对比较长的时间，时间成本相对较高，这也是为什么早期的synchronized效率低的原因。庆幸的是在Java6之后Java官方从JVM层面对synchronized进行了较大优化，所以现在的synchronized锁效率也优化得很不错了。Java6之后，为了减少获得锁和释放锁所带来的性能消耗，引入了轻量级锁和偏向锁。详见：[偏向锁、轻量级锁、重量级锁][2]
![][image-3]

[1]:	ulysses://x-callback-url/open?id=9BX-MsWKIEROne-bD04B-w
[2]:	ulysses://x-callback-url/open?id=e0zmPAyfKSWMZVFswCSEQA "偏向锁、轻量级锁、重量级锁"

[image-3]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191006125013.jpg