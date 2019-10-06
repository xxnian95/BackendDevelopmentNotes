# Monitor机制
monitor的基本元素：
1. 临界区
2. monitor 对象及锁
3. 条件变量以及定义在 monitor 对象上的 wait，signal 操作。

使用 monitor 机制的**目的**主要是为了**互斥进入临界区**，为了做到能够阻塞无法进入临界区的进程/线程，还需要一个 monitor object 来协助，这个 monitor object 内部会有相应的数据结构，例如列表，来保存被阻塞的线程；同时由于 monitor 机制本质上是基于 mutex 这种基本原语的，所以 monitor object 还必须维护一个基于 mutex 的锁。

此外，为了在适当的时候能够阻塞和唤醒 进程/线程，还需要引入一个**条件变量**，这个条件变量用来决定什么时候是“适当的时候”，这个条件可以来自程序代码的逻辑，也可以是在 monitor object 的内部，总而言之，程序员对条件变量的定义有很大的自主性。不过，由于 monitor object 内部采用了数据结构来保存被阻塞的队列，因此它也必须对外提供两个 API 来**让线程进入阻塞状态以及之后被唤醒**，分别是 `wait` 和 `notify`。

## monitor object
synchronized需要关联一个对象，这个对象叫做monitor object。Java的Object满足monitor object的要求，因此所有的Java对象都可以作为monitor机制的monitor object。
Java 对象存储在内存中，分别分为三个部分，即**对象头、实例数据和对齐填充**，而在其对象头中，保存了**锁标识**；同时，Object类定义了`wait()`，`notify()`，`notifyAll()`方法，这些方法的具体实现，依赖于一个叫 **ObjectMonitor** 模式的实现，这是 JVM 内部基于`C++`实现的一套机制，基本原理如下所示：
![][image-1]
当一个线程需要获取 Object 的锁时，会被放入 **EntrySet** 中进行等待，如果该线程获取到了锁，成为当前锁的 owner。如果根据程序逻辑，一个已经获得了锁的线程缺少某些外部条件，而无法继续进行下去（例如生产者发现队列已满或者消费者发现队列为空），那么该线程可以通过调用 wait 方法将锁释放，进入 wait set 中阻塞进行等待，其它线程在这个时候有机会获得锁，去干其它的事情，从而使得之前不成立的外部条件成立，这样先前被阻塞的线程就可以重新进入 EntrySet 去竞争锁。这个外部条件在 monitor 机制中称为**条件变量**。

### 竞争
1. 要进入一个 synchronized 方法修饰的方法或者代码块，会先获取与 synchronized 关键字绑定在一起的 Object 的对象锁，这个锁也限定了其它线程无法进入与这个锁相关的其它 synchronized 代码区域。
2. 如果已经有线程成为了synchronized绑定对象Object的owner，那么会在Entry Set当中等待，直到owner释放了Object的锁。

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191006125135.png