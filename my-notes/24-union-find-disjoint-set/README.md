# 24 Union Find (Disjoint Set)

如果问题拆解的时候发现步骤里涉及集合的查询和操作，有的话就可以用并查集.

一开始每个点弄一个集，一开始大家都是自己的root。用hashmap来存值与点的对应关系。查找时记得做path compress，root点内存一下这个集有多少个点，合并的时候把**点少的合并到大的**里边，这样树的高度就会少点？忘了...

**基本实现：**

[L589 Connecting Graph](l589-connecting-graph.md)

[L590 Connecting Graph II](l590-connecting-graph-ii.md)

[L591 Connecting Graph III](l591-connecting-graph-iii.md)

**变形：**

[305 Number of Island II](305-number-of-island-ii.md) (L434) - Union Find

[L137 Graph Valid Tree](l137-graph-valid-tree.md) (261)

[L432 Find Weak Connected Component in the Directed Graph](l432-find-weak-connected-component-in-the-directed-graph.md)

[L477 Surrounded Regions](../graph/l477-surrounded-regions.md) - 也可以bfs或dfs染色来做

[L629 Minimum Spanning Tree](l629-minimum-spanning-tree.md)

547 Friend Circles

**other related :**

[200 Number of Islands](../graph/200-number-of-islands.md) (L433)

[L431 Connected Component in Undirected Graph](../graph/l431-connected-component-in-undirected-graph.md) - can use bfs or dfs as well (323)

\*在有向图中，弱联通的概念跟无向图中的联通一样，有向图中的强连通意思是每两个点之间都有通路，稍微要注意一下。（听说有个用两次dfs来求这的方法 -- 强化班答疑课）
