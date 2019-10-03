# StringBuilder vs. StringBuffer
# 11-Java基础/基础
---
## 线程安全
StringBuffer：线程安全，StringBuilder：线程不安全。因为 StringBuffer 的所有公开方法都是 `synchronized` 修饰的，而 StringBuilder 并没有 StringBuilder 修饰。
```java
``
@Override
public synchronized StringBuffer append(String str) {
	toStringCache = null;
	super.append(str);
	return this;
}
```


## 缓冲区
StringBuffer
[image:9F4AA42A-8B17-4B31-8985-26535EC791AD-972-000013EAF20F87C0/CA48A0FB-F405-4D4C-B401-05A6A502B878.png]()
StringBuilder
[image:69D44F5D-899A-4272-B5DF-201769B6D8B8-972-000013F4C71CCB01/23945E5E-8B81-4E63-A9F9-DF6A6B67FB96.png]()
StringBuffer 每次获取 toString 都会直接使用缓存区的 toStringCache 值来构造一个字符串。
而 StringBuilder 则每次都需要复制一次字符数组，再构造一个字符串。
所以，缓存冲这也是对 StringBuffer 的一个优化吧，不过 StringBuffer 的这个toString 方法仍然是同步的。

## 性能
StringBuilder性能远好于StringBuffer。

