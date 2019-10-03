# Hibernate和MyBatis的对比

## Hibernate优势
1. Hibernate的DAO层开发比MyBatis简单，Mybatis需要::维护SQL和结果映射::。
2. Hibernate对对象的维护和缓存要比MyBatis好，对增删改查的对象的维护要方便。
3. Hibernate数据库移植性很好，MyBatis的数据库移植性不好，::不同的数据库需要写不同SQL::。
4. Hibernate有更好的二级缓存机制，可以使用第三方缓存。MyBatis本身提供的缓存机制不佳。

## MyBatis优势
1. MyBatis可以进行::更为细致的SQL优化::，可以::减少查询字段::，提高效率。
2. MyBatis容易掌握，而Hibernate门槛较高。

## 总结
1. Mybatis和Hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句。
2. Mybatis直接编写原生态sql，可以严格控制sql执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，因为这类软件需求变化频繁，一但需求变化要求迅速输出成果。但是灵活的前提是mybatis无法做到数据库无关性，如果需要实现支持多种数据库的软件，则需要自定义多套sql映射文件，工作量大。
3. Hibernate对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件，如果用Hibernate开发可以节省很多代码，提高效率。
