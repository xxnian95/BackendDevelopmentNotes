# Redis的驱逐策略

## no-eviction
::不删除::策略，达到最大内存限制时，如果需要更多内存，::直接返回错误信息::。 大多数写命令都会导致占用更多的内存（有极少数会例外，如DEL）。

## allkeys-lru
所有key通用； 优先删除::最近最少使用::（less recently used ，LRU） 的 key。
volatile-lru: 只限于设置了::expire::的部分； 优先删除::最近最少使用::（less recently used ，LRU） 的 key。

## volatile-lru
从已设置过期时间的数据集中挑选::最近最少使用::的数据淘汰。

## allkeys-random
所有key通用； ::随机删除::一部分 key。

## volatile-random
只限于设置了*expire*的部分； ::随机删除::一部分 key。

## volatile-ttl
只限于设置了expire的部分； 优先删除::剩余时间（time to live，TTL）::短的key。
