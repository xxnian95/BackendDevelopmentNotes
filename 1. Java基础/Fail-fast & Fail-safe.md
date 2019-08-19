# Fail-fast & Fail-safe
#1. Java基础/容器/其他# 
- - - -
* 如果在使用迭代器的过程中有其他线程修改了 HashMap 就会抛出`ConcurrentModificationException`异常
* `java.util`中所有的集合类都是fail-fast，`java.util.concurrent`中所有的类都是**fail-safe**
* fail-safe的遍历不是直接在集合内容上访问，而是::先复制原有集合内容::，在拷贝的集合上进行遍历。
	* 	java.util.concurrent.ConcurrentMap
	* Collections.concurrentList

[HashMap - 遍历](bear://x-callback-url/open-note?id=2ED42E98-C190-412E-9289-368B555C3794-3969-00002162BF932E81&header=%E9%81%8D%E5%8E%86)