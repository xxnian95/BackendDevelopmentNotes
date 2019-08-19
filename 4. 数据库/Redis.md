# Redis
#4. 数据库/NoSQL# #4. 数据库/Redis#
- - - -
Redis是一个由Salvatore Sanfilippo写的key-value存储系统。Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

## 数据类型
1. String
2. List
3. Hash
4. Set
5. Sorted Set

## 特点
1. **内存**数据库，速度快，也支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
2. Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
3. Redis支持数据的备份，即**master-slave**模式的数据备份。
4. 支持**事务**

## 优势
速度、性能、事务、特性
* **性能极高** – Redis能读的速度是110000次_s,写的速度是81000次_s 。
* **丰富的数据类型** – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
* **原子** – Redis的所有操作都是原子性的，同时Redis还支持事务。
* **丰富的特性** – Redis还支持 publish/subscribe, 通知, key 过期等等特性。
>  Redis非常快，因为它完全基于内存，绝大部分请求是纯粹的内存操作，非常快速。数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)；数据结构简单，对数据操作也简单，Redis中的数据结构是专门进行设计的；**采用单线程**，避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗；使用多路I/O复用模型，非阻塞IO。  

## 持久化
* Redis是内存型数据库，需要将数据持久化到硬盘上。
* Redis默认是只开启RDB，当Redis重启时，它会优先使用AOF文件来还原数据集。
* 两种持久化：RDB、AOF
	* RDB：对Redis中数据执行周期性的持久化
	* AOF：对于每条写入命令作为日志，以append-only模式写入一个日志文件中（更加完整）
	* Redis重启后，通过回放AOF日志中的指令重新构建数据集
	* AOF大到一等程度后，rewrite操作，构建更小的AOF文件
### RDB快照
将某个时间点的所有数据都存放到硬盘上。
	* 优点：（冷备）
		1. 生成多个数据文件，适合冷备。AOF需要写脚本处理。
		2. RDB对Redis性能影响小，fork子进行进行RDB持久化
		3. RDB重启恢复速度快
	* 缺点：
		1. 恢复效果差，数据丢失
		2. fork子进程时，数据文件特别大，会对客户端服务暂停数毫秒
### AOF
append-only-file
> append：附加  
将写命令添加到 AOF 文件（Append Only File）的末尾。
	* 优点：
		1. 保护数据不丢失
		2. append-only写入，没有磁盘寻址开销，写入性能高，文件不易损坏
		3. AOF日志文件过大rewrite对客户端读写性能影响小
		4. 日志文件可读
	* 缺点：
		1. 日志文件大
		2. 支持的写QPS低
		3. 健壮性低

## Redis的rehash过程
有两个hashTable,ht[0]在没有开始rehash时保存数据。如果开始rehash，每一次修改或者查询就将一个在ht[0]中的键值对转移到ht[1]中，在put数据时就直接put到第二个ht中，get数据时先从第一个找，找不到再从第二个找,delete亦然。

## 主从复制
详见 [http://blog.itpub.net/31545684/viewspace-2213629/](http://blog.itpub.net/31545684/viewspace-2213629/) 

[redis分布式一致性hash算法 - LC_HYQ的博客 - CSDN博客](https://blog.csdn.net/qq_32182461/article/details/81737556)
