# Spring的事务

## 事务的传播行为

1. REQUIRED
`PROPAGATION_REQUIRED`表示当前方法**必须运行在事务中**。如果当前事务存在，方法将会在该事务中运行。否则，会启动一个新的事务。

2. SUPPORTS
`PROPAGATION_SUPPORTS`表示当前方法**不需要事务上下文**，但是如果存在当前事务的话，那么该方法会在这个事务中运行。

3. MANDATORY
`PROPAGATION_MANDATORY`表示该方法**必须在事务中运行**，如果当前事务不存在，则会抛出一个**异常**。
mandatory: 强制的，法定的

4. REQUIRED NEW
`PROPAGATION_REQUIRED_NEW`表示当前方法**必须运行在它自己的事务中**。一个新的事务将被启动。如果存在当前事务，在该方法执行期间，当前事务会被挂起。如果使用JTATransactionManager的话，则需要访问TransactionManager。

5. NOT SUPPORTED
`PROPAGATION_NOT_SUPPORTED`表示该方法**不应该运行在事务中**。如果存在当前事务，在该方法运行期间，**当前事务将被挂起**。如果使用JTATransactionManager的话，则需要访问TransactionManager。

6. NEVER
`PROPAGATION_NEVER`表示当前方法**不应该运行在事务上下文中**。如果当前正有一个事务在运行，则会抛出**异常**。

7. NESTED
`PROPAGATION_NESTED`表示如果当前已经存在一个事务，那么该方法将会在**嵌套事务**中运行。嵌套的事务可以独立于当前事务进行单独地提交或回滚。如果当前事务不存在，那么其行为与`PROPAGATION_REQUIRED`一样。


## 事务的实现和管理

### 实现方式
- 编程式事务管理对基于 POJO 的应用来说是唯一选择。我们需要在代码中调用`beginTransaction()`、`commit()`、`rollback()`等事务管理相关的方法，这就是编程式事务管理。
- 基于 TransactionProxyFactoryBean 的声明式事务管理
- 基于 @Transactional 的声明式事务管理
- 基于 Aspectj AOP 配置事务

### 管理方式
Spring并不直接管理事务，而是提供了多种事务管理器，他们将事务管理的职责委托给Hibernate或者JTA等持久化机制所提供的相关平台框架的事务来实现。
Spring事务管理器的接口是`org.springframework.transaction.PlatformTransactionManager`，通过这个接口，Spring为各个平台如JDBC、Hibernate等都提供了对应的事务管理器，但是具体的实现就是各个平台自己的事情了。
从这里可知具体的具体的事务管理机制对Spring来说是透明的，它并不关心那些，那些是对应各个平台需要关心的，所以Spring事务管理的一个优点就是 _为不同的事务API提供一致的编程模型_ ，如JTA、JDBC、Hibernate、JPA。