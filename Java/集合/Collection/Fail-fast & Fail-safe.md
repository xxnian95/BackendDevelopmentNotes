# Fail-fast & Fail-safe
* 如果在使用迭代器的过程中有其他线程修改了 HashMap 就会抛出`ConcurrentModificationException`异常
* `java.util`中所有的集合类都是**fail-fast**，`java.util.concurrent`中所有的类都是**fail-safe**
* fail-safe的遍历不是直接在集合内容上访问，而是::先复制原有集合内容::，在拷贝的集合上进行遍历。
	* java.util.concurrent.ConcurrentMap
	* Collections.concurrentList

产生`ConcurrnetModificationException`的原因：
[Java ConcurrentModificationException异常原因和解决方法 - Matrix海子 - 博客园]()(https://www.cnblogs.com/dolphin0520/p/3933551.html)

# 

