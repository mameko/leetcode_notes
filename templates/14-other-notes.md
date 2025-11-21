# 14 other notes

程序参照[树的模板](shu-de-mo-ban.md)

遍历 -- 自己拿个小本子，一边遍历一边记住，所以不用返回值，但要带小本子（要把arraylist之类的传入子函数继续递归）result在parameter

分治 -- 领导把任务分给小弟们，小弟们返回子问题答案之后，领导结合一下自己的再向上汇报。所以函数会有返回值。result在返回值

DFS 深度优先包括

1 递归实现， 包括：遍历 & 分治

2 非递归实现

回溯：subset里面，传进去时+1，返回以后要改回来-1

求最大值，无解时，返回Integer.MIN\_VALUE, 求最小值，无解时，返回Integer.MAXVALUE。if ( XX == null）return Integer.MIN\_VALUE

如果拿着题目没头绪，而且题目是一个数组而且还是O(n2)的话，可以考虑先排序再看看。

图的算法：

* 连通加权无向图求min cost，有Kruskal和prim算法。例如：修公路连城市，城市之间都能修，要找最小cost的那个。[L629 Minimum Spanning Tree](../my-notes/24-union-find-disjoint-set/l629-minimum-spanning-tree.md)
* Dijkastras，最短路径算法，路由里有用到。OSPF（最短路径优先的link state protocol）就用那个。其实还有什么bellman-ford之类的有空了解下 -- [743 Network Delay Time](../my-notes/23-heap/743-network-delay-time.md) -- [https://blog.csdn.net/v\_JULY\_v/article/details/6181485](https://blog.csdn.net/v_JULY_v/article/details/6181485)

需要额外信息的时候要通过加数据来记录，不然就只能用时间复杂度去换。

Java Singleton: [http://www.runoob.com/design-pattern/singleton-pattern.html](http://www.runoob.com/design-pattern/singleton-pattern.html)
