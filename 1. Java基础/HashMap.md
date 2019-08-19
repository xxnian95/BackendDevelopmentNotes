# HashMap
#1. Java基础/容器/HashMap#	#1. Java基础/容器/Map#
- - - -
## 底层实现
基于动态数组和链表（拉链法）。链表长度超过8则会变为**红黑树**（JDK1.8开始）。

## 扩容
### JDK1.7
**所有元素的hash值都需要重新计算。**
取出数组元素（实际数组索引位置上的每个元素是每个独立单向链表的头部，也就是发生 Hash 冲突后最后放入的冲突元素）然后遍历以该元素为头的单向链表元素，依据每个被遍历元素的 hash 值计算其在新数组中的下标然后进行交换（即原来 hash 冲突的单向链表尾部变成了扩容后单向链表的头部）。
### JDK1.8
1. 只有一半的元素需要改变位置。
按位计算`e.hash&oldCapacity`。如果为0，索引不变。如果为1，索引为oldCapacity+原索引位。
因为 hash 值本来就是随机性的，所以 hash 按位与上 newTable 得到的 0（扩容前的索引位置）和 1（扩容前索引位置加上扩容前数组长度的数值索引处）就是随机的，所以扩容的过程就能把之前哈西冲突的元素再随机的分布到不同的索引去。
2. 计算hashmap的方式改为：高16位异或低16位。
## 遍历
HashMap有四种遍历方式
1. `for each map.entrySet()`
2. 调用`map.entrySet()`的集合迭代器
3. `for each map.keySet()`
4. 临时变量保存`map.entrySet()`，与1类似

* 对于 keySet 实质上是遍历了两次，一次是转为 iterator 迭代器遍历，一次就从 HashMap 中取出 key 所对于的 value 操作（通过 key 值 hashCode 和 equals 索引）
* entrySet 方式只遍历了一次，它把 key 和 value 都放到了 Entry 中，所以效率高。
* HashMap是Faile-fast [Fail-fast & Fail-safe](bear://x-callback-url/open-note?id=0A1026DE-F35D-40B0-9780-DD6D77612FA7-3969-000022A0E490EE27)

## 线程安全
### 首尾相接的死循环
JDK1.7 中并发扩容操作可能会导致哈希碰撞的链表结构为循环链表，从而导致在后续 put、get 操作时发生**死循环**。
而对于 JDK1.8 中扩容链表的顺序是不会发生逆向的，所以自然怎么遍历都不会出现循环链表的情况，故 JDK1.8 中不会出现并发循环链表，但由于 JDK1.7 与 JDK1.8 中都是无锁保护的，所以依然是并发不安全的。
### 丢失修改信息
JDK1.7中，发生碰撞后，新的元素会放在链表的头部。如果两个线程同时添加元素，则均会返回当前元素作为头结点的链表，因此另一个线程的修改就会丢失。
### 如何线程安全地使用HashMap？
1. HashTable
2. ConcurrentHashMap
3. SynchronizedMap