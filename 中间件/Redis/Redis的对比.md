# Redis的对比
## Redis vs. memecache
- memcached所有的值均是::简单的字符串::，redis作为其替代者，支持更为丰富的数据类型
- redis的速度比memcached快很多
- redis可以持久化其数据
## Jedis vs.  Redisson
- Jedis是Redis的Java实现的客户端，其API提供了比较全面的Redis命令的支持。
- Redisson实现了分布式和可扩展的Java数据结构，和Jedis相比，功能较为简单，不支持字符串操作，不支持排序、事务、管道、分区等Redis特性。Redisson的宗旨是促进使用者对Redis的关注分离，从而让使用者能够将精力更集中地放在处理业务逻辑上。