# JVM内存

![][image-1]


- **程序计数器**：知道线程执行位置，保证线程切换后能恢复到正确的执行位置。
- **虚拟机栈**：存栈帧。栈帧里存**局部变量表、操作栈、动态连接、方法返回地址**。局部变量表又存了各种基本数据类型和**对象引用（句柄）**。
- **本地方法栈**：为Native方法服务
- **堆**：存放**对象实例、对象类型数据的指针**和**数组**，可以处于物理上不连续的内存空间
- **方法区**：存类信息、**常量**、**静态变量**。有运行时常量池，存放类的符号引用
	* 永久代是方法区的实现，但是JDK1.8后，永久代已被Metaspace元空间所取代。**元空间并不在虚拟机中，而是在本地内存中。**原因：
		1. 字符在永久代中容易溢出
		2. 永久代的GC效率低
只有**堆和方法区**会进行GC。



![][image-2]

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191020223758.png
[image-2]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191005120357.png