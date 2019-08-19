# synchronized & ThreadLocal
#2. 并发/synchronized#
- - - -
* Synchronized关键字主要解决多线程共享数据同步问题；ThreadLocal主要解决多线程中数据因并发产生不一致问题。
* Synchronized是利用锁的机制，使变量或代码块只能被一个线程访问。而ThreadLocal为每一个线程都提供变量的副本，使得每个线程访问到的并不是同一个对象，这样就隔离了多个线程对数据的数据共享。
