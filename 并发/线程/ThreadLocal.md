# ThreadLocal
简单说ThreadLocal就是一种以::空间换时间::的做法，在每个Thread里面维护了一个以开地址法实现的ThreadLocal.ThreadLocalMap，把数据进行隔离，数据不共享，自然就没有线程安全方面的问题了。每一个线程都可以独立的改变自己的副本，而不会影响其他的线程对应的副本。
![]()
![][image-2]
ThreadLocal用Entry存储键值对。Entry的Key只能是ThreadLocal类，继承自WeakReference，只能存活到下次GC。

---- 
## synchronized vs. ThreadLocal
* Synchronized关键字主要解决多线程共享数据同步问题；ThreadLocal主要解决 _多线程中数据因并发产生不一致问题_ 。
* Synchronized是利用锁的机制，使变量或代码块只能被一个线程访问。而ThreadLocal为每一个线程都提供变量的副本，使得每个线程访问到的并不是同一个对象，这样就 _隔离了多个线程对数据的数据共享_ 。

[image-2]:	https://i.loli.net/2019/10/04/3svHkcF1yxrST4Z.png