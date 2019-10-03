# Hibernate

## 优势
* 对JDBC访问数据库的代码做了::封装::，大大简化了数据访问层繁琐的重复性代码。
	* Hibernate是一个基于JDBC的主流持久化框架，是一个优秀的*ORM实现*。他很大程度的::简化DAO层::的编码工作。
	* Hibernate使用Java::反射::机制，而不是字节码增强程序来实现::透明性::。
	* Hibernate的::性能非常好::，因为它是个轻量级框架。映射的灵活性很出色。它支持各种关系数据库，从一对一到多对多的各种复杂关系。

## ORM
对象-关系映射（Object-Relational Mapping，简称ORM），面向对象的开发方法是当今企业级应用开发环境中的主流开发方法，关系数据库是企业级应用环境中永久存放数据的主流数据存储系统。对象和关系数据是业务实体的两种表现形式，业务实体在内存中表现为对象，在数据库中表现为关系数据。内存中的对象之间存在关联和继承关系，而在数据库中，关系数据无法直接表达多对多关联和继承关系。因此，对象-关系映射(ORM)系统一般以::中间件::的形式存在，主要实现::程序对象到关系数据库数据的映射::。

## 工作原理
1. 通过`Configuration config = new Configuration().configure();`读取并解析hibernate.cfg.xml配置文件
2. 由hibernate.cfg.xml中的`<mapping resource=“com/xx/User.hbm.xml”/>`读取并解析映射信息
3. 通过`SessionFactory sf = config.buildSessionFactory();`创建SessionFactory
4. `Session session = sf.openSession();`打开Sesssion
5. `Transaction tx = session.beginTransaction();`创建并启动事务Transation
6. persistent operate操作数据，持久化操作
7. `tx.commit();`提交事务
8. 关闭Session
9. 关闭SesstionFactory

## 缓存机制
::一级缓存::就是 *Session* 级别的缓存，在事务范围内有效是，内置的不能被卸载。::二级缓存::是 *SessionFactory*级别的缓存，从应用启动到应用结束有效。是可选的，默认没有二级缓存，需要手动开启。保存数据库后，缓存在内存中保存一份，如果更新了数据库就要同步更新。
什么样的数据适合存放到第二级缓存中？
- 很少被修改的数据 帖子的最后回复时间
- 经常被查询的数据 电商的地点
- 不是很重要的数据，允许出现偶尔并发的数据
- 不会被并发访问的数据
- 常量数据
扩展：hibernate的二级缓存默认是不支持分布式缓存的。使用 memcahe,redis等中央缓存来代替二级缓存。



