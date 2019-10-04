# 与CAS相关的Java类
## 原子类
- AtomicInteger是支持原子操作的Integer类，保证对其的增加和减少都是原子性的。
- 基于::CAS::实现。
- 底层使用*sun.misc.Unsafe*类实现。这个类包含了大量的对C代码的操作，包括很多直接内存分配以及原子操作的调用，而它之所以标记为非安全的，是告诉你这个里面大量的方法调用都会存在安全隐患，需要小心使用，否则会导致严重的后果，例如在通过unsafe分配内存的时候，如果自己指定某些区域可能会导致一些类似`C++`一样的指针越界到其他进程的问题。

## 如何解决ABA问题
1. 版本号
2. `AtomicStampedReference`
	`AtomicStampedReference`内部维护了::对象值和版本号::。创建`AtomicStampedReference`对象时，需要传入初始值和初始版本号。当对象设置对象值时，必须对象值和状态戳都满足期望值，才会写入成功。
3. `AtomicMarkedReference`