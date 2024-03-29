# rehash

[浅谈Redis中的Rehash机制 - Coding\_QK - CSDN博客][1]

为了hash表的负载因子( ht0.used/ht0.size )维持在一个合理范围之内，当哈希表保存的键值对数量太多或者太少时，程序需要对哈希表的大小进行相应的扩展或者收缩。

扩展和收缩哈希表的工作可以通过执行rehash（重新散列）操作来完成，Redis对字典的哈希表执行rehash的步骤如下：

为字典的ht1哈希表分配空间，这个哈希表的空间大小取决于要执行的操作，以及ht0当前包含的键值对数量（也即是ht0.used属性的值）： 如果执行的是扩展操作，那么ht1的大小为第一个大于等于ht0.used*2的2^n（2的n次方幂）； 如果执行的是收缩操作，那么ht1的大小为第一个大于等于ht0.used的2^n 。
将保存在ht0中的所有键值对rehash到ht1上面：rehash指的是重新计算键的哈希值和索引值，然后将键值对放置到ht1哈希表的指定位置上。
当ht0包含的所有键值对都迁移到了ht1之后（ht0变为空表），释放ht0，将ht1设置为ht0，并在ht1新创建一个空白哈希表，为下一次rehash做准备。
渐进式rehash
考虑到hash表中的键值对可能非常多，如果一次性完成rehash操作，rehash操作过程中可能因为庞大的计算量导致服务器不能正常处理请求，所以rehash操作是分多次渐进完成的。

以下是哈希表渐进式rehash的详细步骤：

为ht1分配空间，让字典同时持有ht0和ht1两个哈希表。
在字典中维持一个索引计数器变量rehashidx，并将它的值设置为0，表示rehash工作正式开始。
在rehash进行期间，每次对字典执行添加、删除、查找或者更新操作时，程序除了执行指定的操作以外，还会顺带将ht0哈希表在rehashidx索引上的所有键值对rehash到ht1，当rehash工作完成之后，程序将rehashidx属性的值增一。
随着字典操作的不断执行，最终在某个时间点上，ht0的所有键值对都会被rehash至ht1，这时程序将rehashidx属性的值设为-1，表示rehash操作已完成。
在渐进式rehash进行期间，字典的删除（delete）、查找（find）、更新（update）等操作会在两个哈希表上进行。新添加到字典的键值对一律会被保存到ht1里面，而ht0则不再进行任何添加操作，这一措施保证了ht0包含的键值对数量会只减不增，并随着rehash操作的执行而最终变成空表。

[1]:	https://blog.csdn.net/cqk0100/article/details/80400811 "浅谈Redis中的Rehash机制 - Coding_QK - CSDN博客"