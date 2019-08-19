# Set和Map的对比
#1. Java基础/容器/容器的对比# #1. Java基础/容器/HashMap##1. Java基础/容器/Set##1. Java基础/容器/Map#
- - - -
## HashTable & HashMap
* HashTable使用**synchronized**进行同步，**HashMap不是线程安全的**。
* HashMap可以插入::null::的键和值，HashTable的key与value均不为null。
* HashTable多一个::遍历方式::：element。
* HashMap的迭代器是**fail-fast**迭代器。
* HashMap _不能保证Map中元素的顺序保持不变。_
* 容量及扩容的区别。
	* HashTable 的默认容量为11，而 HashMap 为 16（安卓中为 4）。
	* Hashtable 不要求底层数组的容量一定是 2 的整数次幂，而 _HashMap 则要求一定为 2 的整数次幂。_
	* Hashtable 扩容时将容量变为原来的 2 倍加 1，而 HashMap 扩容时将容量变为原来的 2 倍。

## HashMap & ConcurrentHashMap & HashTable
* CurrentHashMap加入**分段锁**，多个线程可以访问不同的锁，减小锁的粒度，并发度更高。
* 锁住的是Segment，每一个Segment内部包含多个Entry。当需要使用某个Entry对象时，会先对Segment上锁。
* HashTable用::synchronized::锁住整个表来保证线程安全，效率非常低。

## HashMap & LinkedHashMap
	* LinkedHashMap是HashMap的子类，**唯一区别是在MashMap的基础上增加了 _双向链表_ 和一个顺序模式属性。**
	* LinkedHashMap具备HashMap其他的所有属性。
	* LinkedHashMap有两种访问顺序：1. 按照插入顺序。2. 按照上次访问的顺序，即每次访问完一个就键值对后，将该元素移植LinkedHashMap的结尾，可用于实现**LRU（Least Recently Used**）。

## HashSet & HashMap
	* HashSet**底层基于HashMap实现。**
	* 添加的元素作为key存入Entry

## HashSet & LinkedHashSet
* LinkedHashSet 与 HashSet 的不同之处在于 LinkedHashSet 依靠 LinkedHashMap 维护着一个**顺序链表结构**用来迭代，而 HashSet 依靠 HashMap 维持着一个简单的哈希表存储访问；此外 LinkedHashSet 与 HashSet 没有任何区别，**都支持元素为 null，都是非并发安全 Set**。
* LinkedHashSet 的实质是 **HashSet**，HashSet 的实质是 **HashMap**

## HashSet & TreeSet
* HashSet 是基于 HashMap 实现的，TreeSet 是基于 TreeMap 实现的，而 TreeMap 是一个有序的二叉树，所以 TreeSet 也是一个有序的二叉树，其提供有序的 Set 集合。
> TreeMap基于**红黑树**实现。  
* 此外 HashSet 不能保证集合的迭代顺序且**允许使用 null 元素**，同时还**不是并发安全的**。而 TreeSet **可以保证集合元素迭代有序**，但是元素必须实现 Comparable 接口或 Comparator 接口来保证排序，此外其元素不能为 **null**，同时该集合也是**非并发安全**的。


