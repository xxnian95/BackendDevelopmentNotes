# ConcurrentHashMap
## JDK1.7
* 介于 HashMap 与 Hashtable 之间。内部采用"锁分段”机制替代Hashtable的独占锁,进而提高性能。
* ConcurrentHashMap 类中包含两个静态内部类 ::HashEntry:: 和 ::Segment::。HashEntry 用来封装映射表的键值对；Segment 用来充当锁的角色，每个 Segment 对象守护整个散列映射表的若干个桶。每个桶是由若干个 HashEntry 对象链接起来的链表。一个 ConcurrentHashMap 实例中包含由若干个 Segment 对象组成的数组。HashEntry 用来封装散列映射表中的键值对。在 HashEntry 类中，key，hash 和 next 域都被声明为 final 型，value 域被声明为 volatile 型。
::Segment 类继承于 ReentrantLock 类::，从而使得 Segment 对象能充当锁的角色。每个 Segment 对象用来守护其（成员对象 table 中）包含的若干个桶。
[synchronized & ReentrantLock]()(bear://x-callback-url/open-note?id=710B8FBC-FE35-424D-BA85-A6377BCA71D4-574-000004A440469659)
---
## JDK1.8
1. 抛弃了原有的分段锁的机制，采用了 _synchronized+CAS_ 来保证并发安全性。
2. 桶内的链表长度大于8时，链表变为红黑树。

