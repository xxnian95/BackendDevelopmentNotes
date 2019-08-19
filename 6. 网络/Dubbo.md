# Dubbo
#6. 网络/RPC#
[Dubbo - 简书](https://www.jianshu.com/p/3090d63e9cb3)
- [ ] 待完善

Dubbo是阿里巴巴开源的基于 Java 的::高性能 RPC（远程调用） 分布式服务框架（SOA::），致力于提供高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案。
内部使用了Netty和Zookeeper，保证了高性能高可用性。

## 为什么要使用Dubbo
	1. 使用Dubbo可以将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，可用于提高业务复用
	2. 灵活扩展，使前端应用能更快速的响应多变的市场需求。
	3. 分布式架构可以承受更大规模的并发流量。

## 对比Spring Cloud
	1. ::通信方式::不同：Dubbo 使用的是 RPC 通信，而Spring Cloud 使用的是**HTTP RESTFul**方式。
	2. ::组成::不一样： dubbo的服务注册中心为Zookeerper，服务监控中心为dubbo-monitor,无消息总线，服务跟踪、批量任务等组件； spring-cloud的服务注册中心为spring-cloud netflix enruka，服务监控中心为spring-boot admin,有消息总线，数据流、服务跟踪、批量任务等组件。

## 内置服务容器
	1. Spring Container
	2. Jetty Container
	3. Log4j Container

## 注册中心
	1. Zookeeper
	2. Redis
	3. Multicast
	4. Simple

## 负载均衡策略
	1. random loadbalance：安权重设置随机概率（默认）；
	2. roundrobin loadbalance：轮寻，按照公约后权重设置轮训比例；
	3. lastactive loadbalance：最少活跃调用数，若相同则随机；
	4. consistenthash loadbalance：一致性hash，相同参数的请求总是发送到同一提供者。



