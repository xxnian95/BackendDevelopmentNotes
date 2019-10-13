# JDK/JRE/JVM

- JDK（Java Development Kit）是针对Java开发人员的产品，是整个Java的核心，包括了J**运行环境JRE、Java工具（编译、开发工具）和Java核心类库**。
- JRE（Java Runtime Environment）是运行Java程序锁必须的环境的集合，包含**JVM标准实现及Java核心类库**。
- JVM（Java Virtual Machine）是整个Java跨平台的最核心的部分，能够运行以Java语言编写的程序。

![][image-1]

字节码是在虚拟机上运行的，而不是编译器。换而言之，是因为JVM能跨平台安装，所以相应JAVA字节码便可以跟着在任何平台上运行。只要JVM自身的代码能在相应平台上运行，即JVM可行，则 JAVA的程序员就可以不用考虑所写的程序要在哪里运行，反正都是在虚拟机上运行，然后变成相应平台的机器语言，而_这个转变并不是程序员应该关心的_。

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191011140408.png