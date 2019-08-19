# Java对象头
#3. JVM/数据结构#
- - - -
在Hotspot虚拟机中，对象在内存中的布局分为三块区域：**对象头（Mark Word、Class Metadata Address）**、实例数据和对齐填充；Java对象头是实现synchronized的锁对象的基础。一般而言，synchronized使用的锁对象是存储在Java对象头里。它是轻量级锁和偏向锁的关键。
![](Java%E5%AF%B9%E8%B1%A1%E5%A4%B4/5982616-6a6fb0174d59f5e4.png)
