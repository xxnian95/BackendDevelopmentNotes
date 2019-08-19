# Object
#1. Java基础/基础#
- - - -
1. protected Object clone() 创建并返回此对象的一个副本。
2. boolean equals(Object obj) 指示某个其他对象是否与此对象“相等”。
3. protected void finalize() 当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。 
4. Class<? extendsObject> getClass() 返回一个对象的运行时类。 
5. int hashCode() 返回该对象的哈希码值。 
6. void notify() 唤醒在此对象监视器上等待的单个线程。 
7. void notifyAll() 唤醒在此对象监视器上等待的所有线程。 能使线程运行
8. String toString() 返回该对象的字符串表示。 
9. void wait() 导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法。 
10. void wait(long timeout) 导致当前的线程等待，直到其他线程调用此对象的 notify() 方法或 notifyAll() 方法，或者超过指定的时间量。 
11. void wait(long timeout, int nanos) 导致当前的线程等待，直到其他线程调用此对象的 notify()
