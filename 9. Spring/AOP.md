# AOP
#9. Spring/AOP#
- - - -
Aspect Orient Programming 面向切面编程
* AOP把软件系统分为两个部分：核心关注点和横切关注点。 _AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。_
* Spring AOP使用的**动态代理**，在每次运行时生成AOP代理对象。所谓的动态代理就是说AOP框架不会去修改字节码，而是在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。
* Spring AOP中的动态代理主要有两种方式，JDK动态代理和CGLIB动态代理。JDK动态代理通过**反射**来接收被代理的类，并且要求被代理的类必须实现一个接口。JDK动态代理的核心是InvocationHandler接口和Proxy类。
* 如果目标类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意，CGLIB是通过**继承**的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。

## JDK动态代理
* JDK动态代理通过**反射**来接收被代理的类，并且要求::被代理的类必须实现一个接口::。JDK动态代理的核心是InvocationHandler接口和Proxy类。

## CGLIB动态代理
* Code Generation Library
* 是一个代码生成的类库，可以在运行时动态的生成某个类的子类。
* CGLIB是通过**继承**的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。

## 静态代理
静态代理是在**编译时**进行织入或类加载时进行织入，比如::AspectJ::。