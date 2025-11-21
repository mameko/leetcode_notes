# 用户系统设计

**Senario**：日活100M

读：100M X 100 / 86400 ～ 100k，peak = 300k，每天大概看100次，这个包括你打开page就会读一下你的好友和自己的资料什么的。

写：100M X 0.1 / 86400 ～ 100， peak = 300，修改自己资料，注册，之类的

是一个读多写少的系统------这时必须想到cache优化读

**Service**：

* 一个AuthService负责登录注册
* 一个UserService负责用户信息存储于查询
* 一个FriendshipService负责好友关系存储/查询

**Storage**：

AuthService，如何记录用户登录信息。需要一个session table：

| session\_key | string(全局唯一）建索引           |
| ------------ | ------------------------- |
| user\_id     | foreign key（指向User table） |
| expire\_at   | timestamp (什么时候过期）        |

多机器登录功能实现：session Table里存两条。iphone登录的session\_id，user1,存一条；然后web登录的session\_id，user1，再存一条。

**FriendshipService**:

* 这个得分双向好友还是单向好友关系。也分sql和noSql。如果用sql，要考虑存一份还是存两份，例如A->B，B->A。建议存两份，然后在一个column建立索引。花点时间，写两份，读起来快。存一份的话每次要两条而且要建两个索引。
* 这个会有consistency的问题，解决方法可以是一周以后统计一下，把数据补全，eventually consistency

| from | foreign（用户)    |
| ---- | -------------- |
| to   | foreign(被关注的人） |

* 如果存nosql的话要存两份，因为要支持查询“A关注了谁？”和“谁关注了A？” 把A作为key，value就是list of follower，另一个也是A作为key，value是list of following。如果是双向好友关系的话，一个表就ok了，因为follower==following。
* 另外如果我们有新需求“要查最近一天关注的好友”，这样就得用casandra这种有rowkey，columnkey和value的了，因为要支持range query（加时间戳index）。同样，如果要快速查到好友，和最近一天的好友，我们可以存两份。（nosql表好简单粗暴，要快就多存几分）
* 存一份的时候，为什么要把小的放左边呢？这个对"查询A用户的所有好友”没有优化，优化的是，“查询A与B是不是好友”这样你就只用查小的那个就ok了。
* 存一份的优化可以是，挡一个cache，用cache优化，可以在cache里，正向存一个记录，反向存一个记录。

**Scale**：

vertical sharding vs horizontal sharding

single point of failure：replica，重要的事情写3份，还可以增加读的copy
