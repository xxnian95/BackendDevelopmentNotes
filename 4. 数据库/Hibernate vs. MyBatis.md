# Hibernate vs. MyBatis
#4. 数据库/MyBatis#
- - - -
Hibernate优势
1. Hibernate的DAO层开发比MyBatis简单，Mybatis需要::维护SQL和结果映射::。
2. Hibernate对对象的维护和缓存要比MyBatis好，对增删改查的对象的维护要方便。
3. Hibernate数据库移植性很好，MyBatis的数据库移植性不好，::不同的数据库需要写不同SQL::。
4. Hibernate有更好的二级缓存机制，可以使用第三方缓存。MyBatis本身提供的缓存机制不佳。

MyBatis优势
1. MyBatis可以进行::更为细致的SQL优化::，可以::减少查询字段::，提高效率。
2. MyBatis容易掌握，而Hibernate门槛较高。