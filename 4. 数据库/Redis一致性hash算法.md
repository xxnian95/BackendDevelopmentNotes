# Redis一致性hash算法
#4. 数据库/Redis#
- - - -
## 一般算法
对对象先hash然后对redis数量取模，如果结果是0就存在0的节点上。1、2同上，假设有0-3四个redis节点、20个数据：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/8F834E0C-71DB-4B09-86EB-DA0DD5927CBB.png)
进行取模后分布如下：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/E8206CC3-69D0-4F36-A54E-7938820DFEDA.png)
现在因为压力过大需要扩容，增加一台redis4、第五个节点：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/97144F23-C886-4328-8CE3-E5A120B342CD.png)
现在只有4个节点还能够命中。命中率是：4/20 = 20%,命中率极其低下。
- - - -
## Redis consistent hashing
环形hash空间：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/5EDDFA51-8EC2-471D-9435-39CF61220FCC.png)
把对象映射到0-2的32次幂减1的空间里。
现在假设有4个对象：object1-object4，将四个对象hash后映射到环形空间中：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/3632EF2D-360D-41BA-BD95-D1ACA282D1A4.png)
接下来把cache映射到hash空间（基本思想就是讲对象和cache都映射到同一hash数值空间中，并且使用相同的hash算法，可以使用cache的ip地址或者其他因子）,假设现在有三个cache：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/EBFF6198-8A7E-487A-9FBA-D6E62F636CBE.png)
每个key顺时针往下走，找到的第一个cache节点就是存储位置：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/39666B3A-6B34-4311-9E80-EE420A6ACF43.png)
现在移除一个cacheB节点、这时候key4将找不到cache，key4继续使用一致性hash算法运算后算出最新的cacheC，以后存储与读取都将在cacheC上：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/955E3DFB-C286-4A89-BD9E-284F8D6D9ECE.png)
移除节点后的影响范围在该节点逆时针计算到遇到的第一个cache节点之间的数据节点。
现在看一下增加一个节点：
![](Redis%E4%B8%80%E8%87%B4%E6%80%A7hash%E7%AE%97%E6%B3%95/083CEEE3-8FC8-4E75-816C-CDB0FDF8A472.png)
影响范围为：添加节点逆时针遇到的第一个cache节点之间的数据节点。



