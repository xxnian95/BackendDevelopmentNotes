## Minor GC、Major GC、Full GC
* Minor GC：回收**新生代**，因为新生代对象存活时间很短，因此*Minor GC会频繁执行*，执行的速度一般也会比较快。::复制::
* Full GC：回收**老年代和新生代**，老年代对象其存活时间长，因此Full GC很少执行，执行速度会比Minor GC慢很多。::标记-整理/清除::

Major GC 是清理::老年代::。
Full GC 是清理::整个堆空间::—包括年轻代和老年代。
---- 
### Minor GC的触发条件
Eden空间满了，就触发一次GC。

### Full GC的触发条件
1. 调用`System.gc()`
	* 建议虚拟机执行GC，但是不一定会执行
		* 老年代空间不足
	2. 空间分配担保失败
	使用复制算法的Minor GC每次执行需要老年代的内存空间作为担保，如果担保失败会执行一次Full GC。
	3. JDK1.7及以前的*永久代*不足
	在 JDK 1.7 及以前，HotSpot 虚拟机中的*方法区*是用永久代实现的，永久代中存放的为一些 *Class 的信息、常量、静态变量*等数据。
	4. Concurrent Mode Failure
