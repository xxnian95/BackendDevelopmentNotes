# ArrayList

## 定义

![][image-1]

```java
public class ArrayList<E> extends AbstractList<E> implements List<E>,RandomAccess,Cloneable,java.io.Serializable
```

- ArrayList支持泛型
- `implements List`：实现了List，即实现了所有可选列表的操作。
- `implements RandomAccess`：表明ArrayList支持快速（通常是固定时间）随机访问。
- `implements Cloneable`：表明ArrayList可以使用`clone()`方法来返回实例的field-for-field拷贝。
- `Serializable`：有序列化功能。


## Fields

默认初始化容量
```java
private static final int DEFAULT_CAPACITY = 10;
```

如果ArrayList的容量为0，则返回该空数组。
```java
private static final Object[] EMPTY_ELEMENTDATA = {};
```


---- 

## 字段

- `long serialVersionUID`
- `int DEFAULT_CAPACITY = 10` 默认容量
- `Object[] EMPTY_ELEMENTDATA` Shared empty array instance used for empty instances.
- `Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA` Shared empty array instance used for default sized empty instances. We distinguish this from `EMPTY_ELEMENTDATA` to know how much to inflate when first element is added.
- `int size`

## Constructor

### 指定初始容量

```java
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}
```


### 使用默认的初始容量

```java
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

```

---- 

## 线程安全

如果存在线程安全问题，ArrayList最好在创建的时候就用`Collections.synchronizedList`方法对ArrayList进行封装：
```java
List list = Collections.synchronizedList(new ArrayList(…));
```

ArrayList是Fail-fast机制：在创建迭代器后，除非_通过迭代器自身的`remove`或`add`方法从结构上对列表进行修改_，否则任何的修改，迭代器都会抛出`ConcurrentModificationException`。

---- 

## 扩容

扩容的流程：
1. 进行空间检查，决定是否进行扩容，以及确定最少需要的容量
2. 如果确定扩容，就执行`grow(int minCapacity)`，minCapacity为最少需要的容量
3. 第一次扩容，逻辑为`newCapacity = oldCapacity + (oldCapacity >> 1)`;即在原有的容量基础上增加一半。
4. 第一次扩容后，如果容量还是小于minCapacity，就将容量扩充为minCapacity。
5. 对扩容后的容量进行判断，如果大于允许的最大容量`MAX_ARRAY_SIZE`，则将容量再次调整为`MAX_ARRAY_SIZE`。至此扩容操作结束。

---- 

## 删除指定索引的元素

1. 检查索引是否越界。如果参数指定索引`index>=size`，抛出一个越界异常
2. 将索引大于index的元素左移一位（左移后，该删除的元素就被覆盖了，相当于被删除了）。
3. 将索引为`size-1`处的元素置为null（为了让GC起作用）。

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191020000411.png