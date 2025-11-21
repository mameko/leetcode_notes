# 12 graph （BFS）

这里主要是讲些图相关的bfs和dfs题，还有就是topological sort了。bfs主要就是用个Queue，dfs就是递归搜索了。BFS的时间复杂度都是O(m+n)，m是边数，n是点数

技巧：

1. 如果是矩阵的话可以用 i \* m + j来避免用pair class
2. 上面那个其实很难记住，某次pramp的时候，某亚洲面孔小哥哥，指了条名路。可以用int\[] .Queue\<int\[]> q = new LinkedList<>(); q.add(new int\[]{i, j});
3. 搜的时候可以考虑用direction matrix， int\[] x = {0, 0, 1, -1}, int\[] y = {1, -1, 0, 0}，然后loop k 从 0 -- 4
4.  如果是扫雷那样8个方向的话，用这个

    ```
        int[] x = {0, 0, -1, 1, 1, 1, -1, -1};
        int[] y = {-1, 1, 0, 0, 1, -1, 1, -1};
    ```

**BFS / DFS :**

[L477 Surrounded Regions](l477-surrounded-regions.md) - 也可以用union find来做\&DFS (130)

[200 Number of Islands](200-number-of-islands.md) (L433) - BFS/DFS/Union Find

[695 Max Area of Island](695-max-area-of-island.md) -- 跟[200 Number of Islands](200-number-of-islands.md)几乎一样，还是bfs/dfs/union find都能解

[694 Number of Distinct Islands](694-number-of-distinct-islands.md)

[711 Number of Distinct Islands II](711-number-of-distinct-islands-ii.md)

[302 Smallest Rectangle Enclosing Black Pixels](../17-binary-search/302-smallest-rectangle-enclosing-black-pixels.md) - 还可以用bfs，不过2分比较好 (L600)

[133 Clone Graph](133-clone-graph.md) --与[138 Copy LIst with Random Pointer](../18-linkedlist/138-copy-list-with-random-pointer.md)有点关系

399 Evaluate Division

[301 Remove Invalid Parentheses](301-remove-invalid-parentheses.md)

[286 Walls and Gates](286-walls-and-gates.md)

417 Pacific Atlantic Water Flow

[317 Shorest Distance from All Buildings](317-shorest-distance-from-all-buildings.md) (L573 Build Post Office II)

490 The Maze

505 The Maze II

499 The Maze III

332 Reconstruct Itinerary

[L531 Six Degrees](l531-six-degrees.md)

[L431 Connected Component in Undirected Graph](l431-connected-component-in-undirected-graph.md) - can use bfs or dfs or union find as well （求联通块里具体有什么node） (323 Number of Connected Components in an Undirected Graph -- 求联通块个数)

[L618 Search Graph nodes](l618-search-graph-nodes.md)

[L611 Knight Shortest Path](l611-knight-shortest-path.md)

[L598 Zombie in Matrix](l598-zombie-in-matrix.md)

[994 Rotting Oranges](994-rotting-oranges.md)

[529 Minesweeper](529-minesweeper.md)

[785 Is Graph Bipartite](785-is-graph-bipartite.md)

[Detect Cycle in directed graph](detect-cycle-in-directed-graph.md) -- dfs

[Detect Cycle in undirected graph](detect-cycle-in-undirected-graph.md) -- dfs / union find

[419 Battleships in a Board](419-battleships-in-a-board.md)

[PrintShortestPath](printshortestpath.md) - bfs with backtrack, Uber

[743 Network Delay Time](../23-heap/743-network-delay-time.md) -- Dijkstra （BFS）

[1631 Path With Minimum Effort](1631-path-with-minimum-effort.md)

[542 01 Matrix](542-01-matrix.md) -- 还有dp解法

[1091 Shortest Path in Binary Matrix](1091-shortest-path-in-binary-matrix.md)

[733 Flood Fill](733-flood-fill.md)

**Special BFS node：**

[279 Perfect Squares](../backpacks/279-perfect-squares.md) -- 之前是用背包解，然后数学解法是四平方定理，然后还能递归，还有另一种DP解法。太高级，有空再补

[127 Word Ladder](127-word-ladder.md) (L120)

[126 Word Ladder II](126-word-ladder-ii.md) (L121)

[752 Open the Lock](752-open-the-lock.md) -- [127 Word Ladder](127-word-ladder.md)变体

**Topological Sort:**

[L127 Topological Sort](l127-topological-sort.md)

[269 Alien Dictionary](269-alien-dictionary.md)

[207 Course Schedule](207-course-schedule.md) (L615)

[210 Course Schedule II](210-course-schedule-ii.md) (L616)

[444 Sequence Reconstruction](444-sequence-reconstruction.md) (L605)

[310 Minimum Height Trees](310-minimum-height-trees.md)

**曼哈顿距离:**

296 Best Meeting Point&#x20;

[573 Squirrel Simulation](573-squirrel-simulation.md)&#x20;

**other related：**

[305 Number of Island II](../24-union-find-disjoint-set/305-number-of-island-ii.md) (L434) - Union Find

407 Trapping Rain Water II -- heap

[L432 Find Weak Connected Component in the Directed Graph](../24-union-find-disjoint-set/l432-find-weak-connected-component-in-the-directed-graph.md) -- Union Find

[L574 Build Post Office](l574-build-post-office.md) -- similar to 296 Best Meeting Point, 这题不一定有解，所以不能用296的方法-- 曼哈顿距离
