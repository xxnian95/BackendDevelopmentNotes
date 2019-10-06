# JDK各版本新特性

## JDK1.8
1. 接口的默认方法
Java 8允许我们给接口添加一个非抽象的方法实现，只需要使用 default关键字即可，这个特征又叫做扩展方法。
2. Lambda 表达式
在Java 8 中你就没必要使用这种传统的匿名对象的方式了，Java 8提供了更简洁的语法Lambda表达式
```
`
Collections.sort(names, (String a, String b) -> {
    return b.compareTo(a);
});
```
对于函数体只有一行的可以简写为：
```java
`Collections.sort(names,(String a, String b) -> b.compareTo(a));
```
3. 函数式接口
 Lambda表达式是如何在java的类型系统中表示的呢？每一个 [lambda表达式][1] 都对应一个类型，通常是接口类型。而“函数式接口”是指仅仅只包含一个抽象方法的接口，每一个该类型的lambda表达式都会被匹配到这个抽象方法。因为 默认方法 不算抽象方法，所以你也可以给你的函数式接口添加默认方法。
 
4. 方法与构造函数引用
Java 8 允许你使用 `::` 关键字来传递方法或者构造函数引用，上面的代码展示了如何引用一个静态方法，我们也可以引用一个对象的方法：
```
`
```
converter = something::startsWith;
String converted = converter.convert(“Java”);
System.out.println(converted);
```
`
```
5. 多重注解

---- 
## JDK1.9
1. Java 平台级模块系统
当启动一个模块化应用时， JVM 会验证是否所有的模块都能使用，这基于requires语句——比脆弱的类路径迈进了一大步。模块允许你更好地强制结构化封装你的应用并明确依赖。
2. Linking
当你使用具有显式依赖关系的模块和模块化的 JDK 时，新的可能性出现了。你的应用程序模块现在将声明其对其他应用程序模块的依赖以及对其所使用的 JDK 模块的依赖。为什么不使用这些信息 _创建一个最小的运行时环境，其中只包含运行应用程序所需的那些模块_ 呢？ 这可以通过 Java 9 中的新的 `jlink` 工具实现。你可以创建针对应用程序进行优化的最小运行时映像而不需要使用完全加载 JDK 安装版本。
3. JShell : 交互式 Java REPL
许多语言已经具有交互式编程环境，Java 现在加入了这个俱乐部。您可以从控制台启动 jshell ，并直接启动输入和执行 Java 代码。 jshell 的即时反馈使它成为探索 API 和尝试语言特性的好工具。
REPL：Read-Eval-Print Loop
4. 改进的 Javadoc
Javadoc 现在支持在 API 文档中的进行搜索。另外，Javadoc 的输出现在符合兼容 HTML5 标准。此外，你会注意到，每个 Javadoc 页面都包含有关 JDK 模块类或接口来源的信息。
5. 集合工厂方法
通常，您希望在代码中创建一个集合（例如，List 或 Set ），并直接用一些元素填充它。 实例化集合，几个 “add” 调用，使得代码重复。 Java 9，添加了几种集合工厂方法：
```
`
```
Set\<Integer\>  ints = Set.of(1,2,3);
List\<String\>  strings = List.of(“first”,”second”);
```
`
```
除了更短和更好阅读之外，这些方法也可以避免您选择特定的集合实现。 事实上，从工厂方法返回已放入数个元素的集合实现是高度优化的。这是可能的，因为它们是不可变的：在创建后，继续添加元素到这些集合会导致 “UnsupportedOperationException” 。
6. 改进的 Stream API
长期以来，Stream API 都是 Java 标准库最好的改进之一。通过这套 API 可以在集合上建立用于转换的申明管道。在 Java 9 中它会变得更好。Stream 接口中添加了 4 个新的方法：dropWhile、takeWhile、ofNullable。还有个 iterate 方法的新重载方法，可以让你提供一个 Predicate (判断条件)来指定什么时候结束迭代：
`IntStream.iterate(1, i -> i < 100, i -> i + 1).forEach(System.out::println);`
第二个参数是一个 Lambda，它会在当前 IntStream 中的元素到达 100 的时候返回 true。因此这个简单的示例是向控制台打印 1 到 99。
除了对 Stream 本身的扩展，Optional 和 Stream 之间的结合也得到了改进。现在可以通过 Optional 的新方法stram将一个 Optional 对象转换为一个(可能是空的) Stream 对象：
`Stream<Integer>  s = Optional.of(1).stream();`
在组合复杂的 Stream 管道时，将 Optional 转换为 Stream 非常有用。
7. 接口私有方法
8. HTTP/2
9. 多版本兼容Jar
---- 
## JDK10
1. 局部变量类型推断
`List<Integer> list = new ArrayList<>();`
可以替换为
` var list = new ArrayList<>();`
2. GC改进和内存管理
JDK 10中有2个JEP专门用于改进当前的垃圾收集元素。
第一个垃圾收集器接口是（JEP 304），它将引入一个纯净的垃圾收集器接口，以帮助改进不同垃圾收集器的源代码隔离。
预定用于Java 10的第二个JEP是针对G1的并行完全GC（JEP 307），其重点在于通过完全GC并行来改善G1最坏情况的等待时间。 _G1是Java 9中的默认GC_ ，并且此JEP的目标是使G1平行。
3. 线程本地握手（JEP 312）
JDK 10将引入一种在线程上执行回调的新方法，因此这将会很方便能::停止单个线程::而不是停止全部线程或者一个都不停。
4. 备用内存设备上的堆分配（JEP 316）
允许HotSpot VM在::备用内存设备::上分配Java对象堆内存，该内存设备将由用户指定。
5. 其他Unicode
6. 基于Java的实验性JIT编译器
7. 根证书
8. 根证书颁发认证
9. 将JDK生态整合单个存储库
10. 删除`javah`


[1]:	https://www.cnblogs.com/knowledgesea/p/3163725.html