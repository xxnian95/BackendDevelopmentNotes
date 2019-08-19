# Minor GC, Major GC, Full GC
#3. JVM/GC#
- - - -
* Minor GC：回收**新生代**，因为新生代对象存活时间很短，因此**Minor GC会频繁执行**，执行的速度一般也会比较快。::复制::
* Full GC：回收**老年代和新生代**，老年代对象其存活时间长，因此Full GC很少执行，执行速度会比Minor GC慢很多。::标记-整理/清除::

## Minor GC的触发条件
Eden空间满了，就触发一次GC。

## Full GC的触发条件
1. 调用**System.gc()**
	* 建议虚拟机执行GC，但是不一定会执行
**2.** **老年代**空间不足
3. 空间分配担保失败
使用复制算法的Minor GC每次执行需要老年代的内存空间作为担保，如果担保失败会执行一次Full GC。
1. JDK1.7及以前的**永久代**不足
在 JDK 1.7 及以前，HotSpot 虚拟机中的**方法区**是用永久代实现的，永久代中存放的为一些 **Class 的信息、常量、静态变量**等数据。
1. Concurrent Mode Failure
 
我们知道在HotSpot虚拟机中存在三种垃圾回收现象，minor GC、major GC和full GC。**对新生代进行垃圾回收叫做minor GC，对老年代进行垃圾回收叫做major GC，同时对新生代、老年代和永久代进行垃圾回收叫做full GC。**许多major GC是由minor GC触发的，所以很难将这两种垃圾回收区分开。major GC和full GC通常是等价的，收集整个GC堆。
