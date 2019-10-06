# ReentrantLock

`ReentrantLock`是Lock唯一的实现类。
`synchronized`和`ReentrantLock`都是**可重入锁**。
## Lock的三大优势
1. 等待可**中断**
	2. 可实现**公平锁**
	3. 可实现选择性**通知**（选择notify哪个线程）
`ReentrantLock`相较于`synchronized`的拓展：
1. `ReentrantLock`可以对获取锁的等待时间进行设置，这样就避免了死锁
	2. `ReentrantLock`可以获取各种锁的信息
	3. `ReentrantLock`可以灵活地实现多路通知
---- 
## 锁的实现
`synchronized`是JVM实现的，`Lock`是JDK实现的。 `ReentrantLock`底层调用的是**Unsafe的park方法**加锁，`synchronized`操作的应该是**对象头中mark word**。
## 性能
如果竞争资源不激烈，两者的性能是差不多的，而当竞争资源非常激烈时（即有大量线程同时竞争），此时`ReentrantLock`的性能要远远优于`synchronized`。
> ReentrantLock的性能曾经优于synchronized，但是synchronized经过优化之后，性能已经不再是选择的标准。synchronized原本是重量级锁，但是JDK1.6加入了偏向锁和轻量级锁。
>  [偏向锁、轻量级锁、重量级锁]()(bear://x-callback-url/open-note?id=17F25E9E-7C5F-48DB-A20C-3673C6E42DC1-1272-00000AB343BF2CAA)

---- 
## Lock提供了比synchronized更多的功能
1. *Lock可以让等待线程只等待一定的时间或者响应中断，Synchronized则是无限等下去。*
2. Lock可以让多个线程只是进行读操作的时候共享锁，Synchronized则是一个线程读操作时，其他线程只能等待。
3. Lock可以*知道线程有没有成功获取到锁*，Synchronized则不行。
4. 但是Lock必须用户*手动写代码释放锁*，如果没有主动释放锁，就有可能导致出现死锁现象，因此**使用Lock时需要在finally块中释放锁**。而synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生。
5. *synchronized是非公平锁*，ReentrantLock既可以是非公平锁也可以是公平锁。
6. ReentrantLock可以绑定多个Condition对象。
7. 可以选择notify哪个线程。

---- 
## 使用选择
除非需要使用ReentrantLock的高级功能，否则*优先使用synchronized*。
1. JVM原生地支持synchronized，ReentrantLock可能因JDK版本而不同。
2. JVM会保证不发生死锁，ReentrantLock需要手动释放锁。
### 以下场景推荐`ReentrantLock`
1. 某个线程在等待锁的控制权的时候**需要中断**
2. 需要分开处理一些`wait-notify`，`ReentrantLock`里面的`Condition`应用，能够控制`notify`哪个线程
3. 具有**公平锁**功能，每个到来的线程都排队等候

