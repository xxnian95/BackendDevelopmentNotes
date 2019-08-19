# MyBatis的缓存
#4. 数据库/MyBatis#
[Mybatis 缓存 - 从简的博客 - CSDN博客](https://blog.csdn.net/qq_42183409/article/details/90740776)
[MyBatis的缓存 - jtlgb - 博客园](https://www.cnblogs.com/jtlgb/p/6037945.html)
- - - -
_MyBatis执行SQL语句之后，这条语句就是被缓存，以后再执行这条语句的时候，会直接从缓存中拿结果，而不是再次执行SQL。_
MyBatis支持**声明式数据缓存（declarative data caching）**。当一条SQL语句被标记为“可缓存”后，首次执行它时从数据库获取的所有数据会被存储在一段高速缓存中，今后执行这条语句时就会从高速缓存中读取结果，而不是再次命中数据库。MyBatis提供了默认下基于Java HashMap的缓存实现，以及用于与OSCache、Ehcache、Hazelcast和Memcached连接的默认连接器。MyBatis还提供API供其他缓存实现使用。

这也就是大家常说的MyBatis::一级缓存::，一级缓存的作用域scope是SqlSession。
MyBatis同时还提供了一种全局作用域global scope的缓存，这也叫做::二级缓存::，也称作全局缓存。

- - - -
## 为什么使用缓存
对于一些我们经常查询的并且不经常改变的数据，如果每次查询都要与数据库进行交互，那么大大降低了效率，因为我们使用缓存，将一些对结果影响不大且经常查询的数据存放在内存中，从而减少与数据库的交互来提高效率，这就是缓存的优势。
- - - -
## Mybatis中的一级缓存
一级缓存指的是Mybatis中`SQLSession`对象的缓存。
当我们在执行查询操作时，查询的结果同时会存入SqlSession提供的一块区域。该区域是一个Map结构，当我们再次查询同样的数据时，MyBatis会先去SQLSession中找，有的话直接拿来用，SQLSession消失后，一级缓存也就消失了。

### 如何清空缓存
使用SqlSession对象调用clearCache()方法  SqlSession.clearCache()。
注意 
看到这可能会有个疑问，如何两次查询一个数据时，中间对这个数据进行了修改，那么第二次查询的结果会不会修改呢？答案是会的。因为在mybatis中 _，除了使用clearCache()方法时会清空缓存，对数据进行 增，删，改，commit()，close()时也会对一级缓存进行清空。_
- - - -
## Mybatis中的二级缓存
二级缓存指的是Mybatis中`SqlSessionFactory`对象的缓存 ，由同一个SqlSessionFactory对象创建的SqlSession共享缓存。

### 配置步骤：
1. 在配置文件 config.xml中配置，让Mybatis支持二级缓存settings 标签中 使用cacheEnabled value 设置为true。
2. 在映射配置文件中 开启支持二级缓存加上<cache/>标签。
3. 让当前操作支持二级缓存，即在select标签 加上 useCache=“true”。
注意：
Mybatis的二级缓存SqlSessionFactory中存放的是是数据而不是对象。
