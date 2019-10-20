# List

 ArrayList & LinkedList
* ArrayList基于*动态数组*实现，LinkedList基于*双向链表*实现。
* ArrayList支持随机访问
* LinkedList在随机位置*添加删除*元素更快
### 具体点：
List 是集合列表接口，ArrayList 和 LinkedList 都是 List 接口的实现类。ArrayList 是 _动态数组顺序表_ ，顺序表的存储地址是连续的，所以 _查找比较快，但是插入和删除时由于需要把其它的元素顺序移动，所以比较耗时_ 。LinkedList 是 _双向链表_ 的数据结构，同时实现了双端队列 Deque 接口，链表节点的存储地址是不连续的，每个存储地址通过指针关联，在查找时需要进行指针遍历节点，所以 _查找比较慢，而在插入和删除时比较快_ 。
---
## Array & ArrayList
* Array可以包含基本类型和对象类型，ArrayList只能包含对象类型。
* Array的大小是固定的，不可以扩容。
* ArrayList提供了更多的方法和特性，例如addAll()、removeAll()、iterator()。
---
## ArrayDeque & LinkedList
* ArrayDeque 和 LinkedList 都实现了 Deque 接口，如果只需要 Deque 接口且从两端进行操作则一般来说 ArrayDeque 效率更高一些，如果同时需要根据索引位置进行操作或经常需要在中间进行插入和删除操作则应该优先选 LinkedList 效率高些，因为 ArrayDeque 实现是*循环数组结构（即一个动态扩容数组，默认容量 16，然后通过一个 head 和 tail 索引记录首尾相连）*，而 LinkedList 是基于双向链表结构的。
---
## Vector & ArrayList
* Vector使用*synchronized*保证线程安全，开销比ArrayList更大，访问速度更慢。最好选择ArrayList而不是Vector。
* Vector每次扩容请求大小是*2倍*空间，ArrayList是*1.5*倍。
---
## Vector & Stack
* Stack 是*继承自 Vector 基于动态数组实现的线程安全栈*，不过现在已经不推荐使用了，Stack 是并发安全的后进先出，实现了一些栈基本操作的方法（其实并不是只能后进先出，因为继承自Vector，所以可以有很多操作，严格说不是一个栈）。其共同点都是使用了方法锁（即 synchronized）来保证并发安全的。
