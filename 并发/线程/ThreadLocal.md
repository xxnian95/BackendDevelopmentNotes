# ThreadLocal

ThreadLocal是JDK底层提供的一个解决多线程并发问题的工具类，它为每个线程提供了一个本地的副本变量机制，实现了和其他线程的隔离，并且这种变量只在本线程的生命周期内起作用，可以减少同一个线程内多个方法之间的公共变量传递的复杂度。

![][image-1]

ThreadLocal用Entry存储键值对。Entry的Key只能是ThreadLocal类，继承自WeakReference，只能存活到下次GC。

## 应用

ThreadLocal主要用于解决两类问题：
1. **并发问题**：使用ThreadLocal代替synchronized来保证线程安全，同步机制采用**空间交换时间**——仅仅提供一份变量，各个线程轮流访问，后者每个线程都持有一份变量，访问时互不影响。
2. 数据存储问题：ThreadLocal为变量在每个 线程中创建了一个副本，所以每个线程可以访问自己内部的副本变量。

### synchronized vs. ThreadLocal

* Synchronized关键字主要解决多线程共享数据同步问题；ThreadLocal主要解决 _多线程中数据因并发产生不一致问题_ 。
* Synchronized是利用锁的机制，使变量或代码块只能被一个线程访问。而ThreadLocal为每一个线程都提供变量的副本，使得每个线程访问到的并不是同一个对象，这样就 _隔离了多个线程对数据的数据共享_ 。

---- 

## 实现原理

### 结构
ThreadLocal主要分为2部分：
1. **成员属性**：主要和计算哈希值相关。
2. **API**：操作内部类。

ThreadLocal并不是把Thread作为`Key`，副本值作为`Value`的一种类似HashMap的结构，而是每个Thread里都有一个`ThreadLocalMap`，ThreadLocal只是操作每个线程的`ThreadLocalMap`而已。

### 原理概述
1. 每个Thread维护着一个ThreadLocalMap的引用
2. ThreadLocalMap是ThreadLocal的内部类，用Entry来进行存储
3. 调用ThreadLocal的set()方法时，实际上就是往ThreadLocalMap设置值，key是ThreadLocal对象，值是传递进来的对象
4. 调用ThreadLocal的get()方法时，实际上就是往ThreadLocalMap获取值，key是ThreadLocal对象
5. ThreadLocal本身并不存储值，它只是作为一个key来让线程从ThreadLocalMap获取value。

### 成员属性

```java
public class ThreadLocal<T> {
    /**
     * 自定义哈希码(ThreadLocalMaps的),降低哈希冲突
     */
    private final int threadLocalHashCode = nextHashCode();

    /**
     * 生成下一个哈希码的hashCode,操作是原子的,从0开始
     */
    private static AtomicInteger nextHashCode =
        new AtomicInteger();

    /**
     * 连续分配的两个ThreadLocal实例的threadLocalHashCode值的增量
     */
    private static final int HASH_INCREMENT = 0x61c88647;

    /**
     * 返回下一个哈希码的hashCode,此方法是一个原子类不停地去加上斐波那契散列数,使得哈希值分布均匀
     */
    private static int nextHashCode() {
        return nextHashCode.getAndAdd(HASH_INCREMENT);
    }

}
```

### 方法

ThreadLocal提供了四个主要的方法：`set`、`get`、`remove`、`initalValue`。

ThreadLocal实例本身不是存储值，而是提供了一个在当前线程中找到副本值的Key（即`ThreadLocalMap`的Key）。

`ThreadLocal`包含在`Thread`中，`get`操作获取`ThreadLocal`中对应的当前线程存储的值的时候，流程如下：
1. 先得到线程的`Thread`对象。
2. 获取此线程对象中维护的`ThreadLocalMap`。
3. 以当前的ThreadLocal为Key，调用ThreadLocalMap的`getEntry`方法获取对应的存储实体e，找到对应的Value，也就是我们想要的此线程对应的ThreadLocal值。

```java
/**
     * 此方法第一次调用发生在当线程通过 {@link #get} 方法访问此线程的ThreadLocal值时
     * 除非线程先调用了 {@link #set}，在这种情况下,{@code initialValue} 方法才不会被这个线程调用
     * 通常情况下,每个线程最多调用1次这个方法,但是也可能再次调用,比如 {@link #remove} 被调用后,调用get
     * 这个方法仅仅简单返回null{@code null}; 如果程序想要它返回除了null之外的初始值,必须继承重写此方法,
     * 通常使用匿名内部类的方式实现
     * @return 返回当前ThreadLocal的初始值 null
     */
    protected T initialValue() {
        return null;
    }

    /**
     * 返回当前线程中保存ThreadLocal的值
     * 如果当前线程没有此ThreadLocal则通过 {@link #initialValue}方法进行初始化值
     * @return 
     */
    public T get() {
        // 获取当前线程对象
        Thread t = Thread.currentThread();
        // 获取此线程对象中维护的`ThreadLocalMap`对象
        ThreadLocalMap map = getMap(t);
        // 如果Map存在
        if (map != null) {
            // 以当前ThreadLocal实例对象为key获取对应的存储实体e
            ThreadLocalMap.Entry e = map.getEntry(this);
            // 找到对应的存储实体e
            if (e != null) {
                @SuppressWarnings("unchecked")
                // 获取e对应的value值,也就是我们想要的当前线程对应此ThreadLocal的值
                T result = (T)e.value;
                return result;
            }
        }
        // 如果map不存在,证明此线程没有维护此ThreadlocalMap对象,进行一波初始化操作
        return setInitialValue();
    }

    /**
     * set的变样实现,用于初始化值initialValue
     * 用来代替防止用户重写set而无法初始化
     * @return 返回初始化后的值
     */
    private T setInitialValue() {
        T value = initialValue();
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        // 如果此map村咋洗,调用map,set设置此实体entry
        if (map != null)
            map.set(this, value);
        else
         // map不存在时,调用此方法进行ThreadLocalMap对象初始化并将此entry作为第一个值放进去
            createMap(t, value);
         // 返回设置的value值s
        return value;
    }

    /**
     * 设置此线程对应的ThreadLocal的值,大多数子类不需要重写此方法,
     * 只需要依赖{@link #initialValue} 方法代替设置当前线程对应的ThreadLocal值
     * @param 将要保存在当前线程对应的ThreadLocal值
     * 1. 获取当前线程`Thread`对象,进而获取此线程对象中维护的`ThreadLocalMap`对象
     *2. 判断当前的`ThreadLocalMap`是否存在,如果存在就直接调用map.set设置entry,如果不存在就调用`createMap`进行`ThreadLocalMap`对象的初始化,并将此实体`entry`作为第一个值存放到`ThreadLocalMap`中。
     */
    public void set(T value) {
        Thread t = Thread.currentThread();
        // 获取当前线程的ThreadLocalMap实例
        ThreadLocalMap map = getMap(t);
        // 存在就设置此entry
        if (map != null)
            map.set(this, value);
        else
        // 不存在就进行对象初始化并设置entry作为第一个值存入ThreadLocalMap中
            createMap(t, value);
    }

    /**
     * 删除当前线程中保存ThreadLocal对应的实体entry
     * 如果此ThreadLocal变量在当前线程中调用{@linkplain #get read} 方法
     * 则会通过调用{@link #initialValue} 方法进行初始化
     * 除非此值value是通过当前线程内置调用set方法设置
     * 这可能导致在当前线程中多次调用initialValue方法初始化
     * 1. 获取当前线程Thread对象，进而获取此线程对象中维护的ThreadLocalMap对象。
     * 2.  判断`ThreadLocalMap`是否存在,如果存在,调用map.remove,以当前的`ThreadLocal`为key删除对应的`entry`
     */
     public void remove() {
         // 获取当前线程Thread对象,进而获取此线程对象中维护的`ThreadLocalMap`对象
         ThreadLocalMap m = getMap(Thread.currentThread());
         if (m != null)
         // 如果ThreadLocalMap存在调用remove方法删除之,当前ThreadLocal对象为key
             m.remove(this);
     }

    /**
     * 获取当前对象Thread对应维护的ThreadLocalMap
     * @param  当前线程
     * @return 对应的ThreadLocalMap
     */
    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }
```

### ThreadLocal的内部类ThreadLocalMap

`ThreadLocalMap`内部利用`Entry`来实现`key-value`的存储，类似`HashMap`的结构，代码如下所示。`Entry`的`key`就是一个`ThreadLocal`，而`value`就是值。

```java
static class Entry extends WeakReference<ThreadLocal<?>> {
     /** The value associated with this ThreadLocal. */
     Object value;

     Entry(ThreadLocal<?> k, Object v) {
         super(k);
         value = v;
     }
 }
```

ThreadLocalMap两个最核心的方法：
1. `getEntry(ThreadLocal<?> key)`
2. `set(ThreadLocal> key, Object value)`

`set()`跟HashMap的`put()`解决哈希冲突的方式不同。`Map#put()`采用的是拉链法，`ThreadLocalMap#set()`采用的是**开放地址法**。此外，`replaceStaleEntry()`和`cleanSomeSlots`可以清除掉`key == null`的实例，防止内存泄漏。

`get()`方法有一个重要的地方，当`key == null`时，调用了`expungeStaleEntry()`方法，有利于GC回收，避免内存泄漏。

```java
private void set(ThreadLocal<?> key, Object value) {

    ThreadLocal.ThreadLocalMap.Entry[] tab = table;
    int len = tab.length;

    // 根据 ThreadLocal 的散列值，查找对应元素在数组中的位置
    int i = key.threadLocalHashCode & (len-1);

    // 采用“线性探测法”，寻找合适位置
    for (ThreadLocal.ThreadLocalMap.Entry e = tab[i];
        e != null;
        e = tab[i = nextIndex(i, len)]) {

        ThreadLocal<?> k = e.get();

        // key 存在，直接覆盖
        if (k == key) {
            e.value = value;
            return;
        }

        // key == null，但是存在值（因为此处的e != null），说明之前的ThreadLocal对象已经被回收了
        if (k == null) {
            // 用新元素替换陈旧的元素
            replaceStaleEntry(key, value, i);
            return;
        }
    }

    // ThreadLocal对应的key实例不存在也没有陈旧元素，new 一个
    tab[i] = new ThreadLocal.ThreadLocalMap.Entry(key, value);

    int sz = ++size;

    // cleanSomeSlots 清楚陈旧的Entry（key == null）
    // 如果没有清理陈旧的 Entry 并且数组中的元素大于了阈值，则进行 rehash
    if (!cleanSomeSlots(i, sz) && sz >= threshold)
        rehash();
}

/**用了开放定址法，所以当前key的散列值和元素在数组的索引并不是完全对应的，首先取一个探测数（key的散列值），如果所对应的key就是我们所要找的元素，则返回，否则调用getEntryAfterMiss()，如下：*/
private Entry getEntryAfterMiss(ThreadLocal<?> key, int i, Entry e) {
    Entry[] tab = table;
    int len = tab.length;

    while (e != null) {
        ThreadLocal<?> k = e.get();
        if (k == key)
            return e;
        if (k == null)
            expungeStaleEntry(i);
        else
            i = nextIndex(i, len);
        e = tab[i];
    }
    return null;
}
```


[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191007171626.png