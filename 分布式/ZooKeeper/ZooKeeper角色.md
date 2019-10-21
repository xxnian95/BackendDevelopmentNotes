# ZooKeeper角色

ZooKeeper集群是一个基于**主从复制**的高可用集群，每个服务器承担如下三种角色之一：
1. Leader
2. Follower
3. Observer

![][image-1]


## Leader

- 一个ZooKeeper集群同一时间内只有一个实际工作的Leader，它会发起并维护与各个Follower、Observer间的心跳。
- 所有的写操作必须要通过Leader完成，再由Leader广播到其他服务器。只要有超过半数节点（不包括Observer）写入成功，该写请求就会被提交（类似于2PC协议）。

## Follower

- 一个ZooKeeper集群可能同时存在多个Follower，它会响应Leader的心跳。
- Follower可以直接处理并返回客户端的**读请求**，但会将**写请求**转发给Leader处理。
- 负责在Leader处理写请求时对请求进行投票。

## Observer

- 角色与Follower类似，但是没有投票权。
- Zookeeper需保证高可用和强一致性，为了支持更多的客户端，需要增加更多Server；Server增多，投票阶段延迟增大，影响性能；引入Observer，Observer不参与投票；Observers接受客户端的连接，并将写请求转发给leader节点；加入更多Observer节点，提高伸缩性，同时不影响吞吐率。

[image-1]:	https://raw.githubusercontent.com/zhangpengnian/ImageRepository/master/img/20191021153915.png