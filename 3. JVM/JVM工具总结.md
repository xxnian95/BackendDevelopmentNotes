# JVM工具总结
#3. JVM/工具#
[JVM命令调优-纯洁的微笑](https://mp.weixin.qq.com/s?__biz=MzI4NDY5Mjc1Mg==&mid=2247483966&idx=1&sn=dfa3375d36aa2c0c25a775522e381e62&chksm=ebf6da41dc815357e0d53c73865a23f41219e75bac5a4d510bfa31cc51594b59a20e2e4f6cb8&scene=21#wechat_redirect)
- - - -
## jps
JVM Process Status Tool，显示指定系统内所有的HotSpot::虚拟机进程::。

## jstat
jstat(JVM statistics Monitoring)是用于::监视虚拟机运行时状态信息::的命令，它可以显示出虚拟机进程中的类装载、内存、垃圾收集、JIT编译等运行数据。

## jmap
* jmap(JVM Memory Map)命令用于::生成heap dump::文件，如果不使用这个命令，还阔以使用-XX:+HeapDumpOnOutOfMemoryError参数来让虚拟机出现OOM的时候·自动生成dump文件。
> Dump文件是进程的内存镜像。可以把程序的执行状态通过调试器保存到dump文件中。  
> Dump文件是用来给驱动程序编写人员调试驱动程序用的，这种文件必须用专用工具软件打开。  
> 当我们的程序发布出去之后，在客户机上是无法跟踪代码的，所以Dump（扩展名是 .dmp）文件对于我们来说特别重要。我们可以通过.dmp文件把出现问题的情况再现，然后根据再现的状况（包括堆栈调用等情况），可以找到出现问题对应的行号。  
* jmap不仅能生成dump文件，还阔以查询finalize执行队列、Java堆和永久代的详细信息，如当前使用率、当前使用的是哪种收集器等。

## jhat
jhat(JVM Heap Analysis Tool)命令是与jmap搭配使用，::用来分析jmap生成的dump::，jhat内置了一个微型的HTTP/HTML服务器，生成dump的分析结果后，可以在浏览器中查看。在此要注意，一般不会直接在服务器上进行分析，因为jhat是一个耗时并且耗费硬件资源的过程，一般把服务器生成的dump文件复制到本地或其他机器上进行分析。

## jstack
jstack用于生成java虚拟机当前时刻的::线程快照::。
线程快照是当前java虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的主要目的是定位线程出现长时间停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等。 线程出现停顿的时候通过jstack来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做什么事情，或者等待什么资源。 如果java程序崩溃生成core文件，jstack工具可以用来获得core文件的java stack和native stack的信息，从而可以轻松地知道java程序是如何崩溃和在程序何处发生问题。
另外，jstack工具还可以附属到正在运行的java程序中，看到当时运行的java程序的java stack和native stack的信息, 如果现在运行的java程序呈现hung的状态，jstack是非常有用的。

## jinfo
jinfo(JVM Configuration info)这个命令作用是::实时查看和调整虚拟机运行参数::。 之前的jps -v口令只能查看到显示指定的参数，如果想要查看未被显示指定的参数的值就要使用jinfo口令。
