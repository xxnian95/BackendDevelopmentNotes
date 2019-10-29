# Collection

![][image-1]

## Set
* *TreeSet*：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet， _HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)_ 。 
* *HashSet*：基于哈希表实现，支持 _快速查找_ ，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历HashSet得到的结果是不确定的。 
* *LinkedHashSet*：具有HashSet的查找效率，且内部使用 _双向链表_ 维护元素的插入顺序。

## List
* *ArrayList*：基于动态数组实现，支持随机访问。 
* *Vector*：和 ArrayList 类似，但它是*线程安全*的。 
* *LinkedList*：基于*双向链表*实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此， LinkedList 还可以用作栈、队列和双向队列。

## Queue
* *LinkedList*：可以用它来实现双向队列。 
* *PriorityQueue*：基于堆结构实现，可以用它来实现优先队列。

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191025083058.png