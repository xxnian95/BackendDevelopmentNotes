# 与CAS相关的类

## 原子类
- AtomicInteger是支持原子操作的Integer类，保证对其的增加和减少都是原子性的。
- 基于::CAS::实现。
- 底层使用*sun.misc.Unsafe*类实现。这个类包含了大量的对C代码的操作，包括很多直接内存分配以及原子操作的调用，而它之所以标记为非安全的，是告诉你这个里面大量的方法调用都会存在安全隐患，需要小心使用，否则会导致严重的后果，例如在通过unsafe分配内存的时候，如果自己指定某些区域可能会导致一些类似`C++`一样的指针越界到其他进程的问题。

## 如何解决ABA问题
1. 版本号
2. `AtomicStampedReference`
	`AtomicStampedReference`内部维护了::对象值和版本号::。创建`AtomicStampedReference`对象时，需要传入初始值和初始版本号。当对象设置对象值时，必须对象值和状态戳都满足期望值，才会写入成功。
3. `AtomicMarkedReference`


## AtomicStampedReference

AtomicStampedReference原子类是一个带有**时间戳**的对象引用，在每次修改后，AtomicStampedReference不仅会设置新值而且还会记录更改的时间。当AtomicStampedReference设置对象值时，_对象值以及时间戳都必须满足期望值才能写入成功_，这也就解决了反复读写时，无法预知值是否已被修改的窘境。

底层实现为：通过Pair私有内部类存储数据和时间戳, 并构造volatile修饰的私有实例。

AtomicStampedReference类的compareAndSet()方法的实现：

1. 同时对当前数据和当前时间进行比较，只有两者都相等是才会执行casPair()方法。
2. 单从该方法的名称就可知是一个CAS方法，最终调用的还是Unsafe类中的compareAndSwapObject方法

