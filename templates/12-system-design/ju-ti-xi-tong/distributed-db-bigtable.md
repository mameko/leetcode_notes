# Distributed DB bigtable

这是设计NoSql key value DB，这里用big table（google）的作为代表，hbase是雅虎的，跟hdfs配套，dynamoDB是亚麻的，Cassandra是FB的(但这个是p2p的）

durability > avaiability > performance

read heavy => B+ tree

write heavy => LSM tree

通常那个replica group会有基数个node，这样那些paxos voting容易点有结果。然后写的时候，假设有3个点，master + 2 slaves。我们写master，master发写到write ahead log，然后slaves写，当其中一个写好以后，master就会报告说写好了。因为majority的node已经得到新数据。选leader的时候也是，要majority的node觉得新leader是leader才会成功。

用heart beat来keep check of the nodes still alive

**Senario**：

数据库就是正删改查（CRUD)也就是读和写，因为设计的是key-value的noSql DB所以不用考虑复杂的查询，都是通过key查一个value出来。

**Service**：

也是很简单，就是提供读和写

增：直接append到最后

删：直接append到最后，把值设成null

改：直接append到最后，把新的值设给key

查：二分查找（详细请看storage）

**Storage**：

我们首先从单机出发，把“key：value”一条一条地写，在硬盘上分块存储，每一块之间没有order，但每一块内部会有序，方便查找。

* 我们选择直接append到最后提高写的速度。
* 首先我们写到内存+Write Ahead Log里，这里是怕断电，我们多写一份WAL。内存直接append
* 然后如果内存够了一块，我们就把这一块排序，然后写到硬盘上。这样就产生那些有序块了。
* 因为优化了写，所以读的时候会慢，我们选择每一块内部建index来提速二分，然后还用bloom filter去快速排出不在块内的key。
* 每一块有一个自己的bloom filter和index，每次从内存开始找，找不到就去硬盘从后往前一块一块读，每块先用bloom filter快速排出一下，如果排不掉，就用index快速找一找。
* 这种直接append的方法时间长了会有很多重复的东西，所以可以进行定期的K路归并整理瘦身。
* 相关概念：外排，外二分查找，350 Intersection of Two Arrays

**Scale**：

* 其实呢，在存内存的那个表的时候，为了能快速append还能快速排序，狗用了一种叫skip list的数据结构。具体得自己拓展阅读。
* index和bloom filter在写硬盘时排序顺便建立。
* 同样，我们通常选择按key水平sharding
* 狗用的是Master + Slave架构
* master能管理slaves的health，heart beat，还能把consistent hashing放那里做。
* 过了一段时间，发现本地硬盘不够存了怎么办？其实，我们后面应该存在GFS里，本地硬盘只作为cache用，因为GFS要过网络，所以会比较慢。好处是，无限大，有replica，还有high availability
* 最后还有一个问题没解决，就是读写加锁的问题。因为读写同一个key时会有race condition。分布式锁可以用zookeeper或者chubby
* 读和写之前要先去lock server拿个锁，拿到了才可以写/读。然后后面的操作就跟之前的一样。
* 因为lock server本来就要存metadata，这里我们可以把consistent hashing功能也放到lock server上，master这时候就只用管理slaves的health
* WAL也可以写到GFS上
