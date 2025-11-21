# 倒排索引

inverted index是搜索引擎的关键，通常我们存东西的时候是

| age | gender | document\_id |
| --- | ------ | ------------ |
| 18  | F      | 1            |
| 20  | F      | 2            |
| 18  | M      | 3            |

然后做成这种形式：（感觉跟typeahead存的时候有点像）

年龄：

| 18 | 【1， 3】 |
| -- | ------ |
| 20 | 【2】    |

性别：

| F | 【1， 2】 |
| - | ------ |
| M | 【3】    |

这时候，搜索引擎会用Trie的数据结构存起key的值（叫trem index）。每个node存起index然后能快速找到term dictionary里的项，每个dict的项存着一条list，list里存的是document的id。这样就可以快速找到某关键词所在的文章了。通常mysql的index是用b+树来存的，也就是只有term dict这一层，这个需要多次random access硬盘。这个trie是存在内存里，然后通过这个index，能快速找到list所在的硬盘位置，不用二分地找。所以快点。

下一步是怎么找出符合多个条件的项呢？例如找18岁的女生。就是用AND操作。然后这是应该想起[349 Intersection of two array](../../../my-notes/two-pointers-other/349-intersection-of-two-arrays.md)。因为查出来的就是两条list，18的一条，然后女生的一条，要找这两个list的intersection，然后那些就是符合多个条件的结果。

虽然这里写的是list，题也只是list的操作，但实际上存储的数据结构是skip list。其实到现在还没十分理解这个东西，只知道是一个多层的链表，上层有写express通道能快点跳过一些值。具体只能参考维基啦。
