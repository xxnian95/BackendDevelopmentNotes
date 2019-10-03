# MyBatis

## 底层原理
1. Mapper 接口在初始SqlSessionFactory 注册的。
2. Mapper 接口注册在了名为 MapperRegistry 类的 HashMap中， key = Mapper class value = 创建当前Mapper的工厂。
3. Mapper 注册之后，可以从SqlSession中get。
4. SqlSession.getMapper 运用了::JDK动态代理::，产生了目标Mapper接口的代理对象。
5. 动态代理的代理类是 MapperProxy ，这里边最终完成了增删改查方法的调用。
MyBatis的**源码**在面试中多次被问到，因此应当有时间看一下。
