# 310 Minimum Height Trees

For an undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

**Format**\
The graph contains`n`nodes which are labeled from`0`to`n - 1`. You will be given the number`n`and a list of undirected`edges`(each edge is a pair of labels).

You can assume that no duplicate edges will appear in`edges`. Since all edges are undirected,`[0, 1]`is the same as`[1, 0]`and thus will not appear together in`edges`.

**Example 1 :**

```
Input:n = 4, edges = [[1, 0], [1, 2], [1, 3]]
        0
        |
        1
       / \
      2   3 
Output:[1]
```

**Example 2 :**

```
Input:n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]
     0  1  2
      \ | /
        3
        |
        4
        |
        5
Output:[3, 4]
```

**Note**:

*   According to the [definition of tree on Wikipedia](https://en.wikipedia.org/wiki/Tree_\(graph_theory\)): “a tree is an undirected graph in which any two vertices are connected by

    exactly one path. In other words, any connected graph without simple cycles is a tree.”
* The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

一开始看到tag是bfs就死往那方面想，然后看了答案才发现自己对这个MHT的理解有误，一开始以为是最靠近叶子的那些点。写了半天还在1441的大数据TLE。其实这题跟topo sort有点像，也跟那题[366 Find Leaves of Binary Tree](../26-tree/366-find-leaves-of-binary-tree.md) 有点像。其实就是把叶子放到queue里，然后一层一层地剥。剥到剩下1个或2个叶子节点时就是答案了。这样的图里其实就有1或2棵那种MHT。

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {        
        if (edges == null || edges.length == 0 || edges[0].length == 0) {
            List<Integer> res = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                res.add(i);
            }
            return res;
        }

        // make the nodes
        List<UndirectedGraph> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new UndirectedGraph(i));
        }

        // connect the edges
        for (int i = 0; i < edges.length; i++) {
            graph.get(edges[i][0]).neighbours.add(graph.get(edges[i][1]));
            graph.get(edges[i][1]).neighbours.add(graph.get(edges[i][0]));
        }

        // put leaves in to queue first
        Queue<UndirectedGraph> queue = new LinkedList<>();
        for (UndirectedGraph node : graph) {
            if (node.neighbours.size() == 1) {
                queue.offer(node);
            }
        }

        // remove leaves layer by layer until only 1 or 2 left
        while (n > 2) {
            int size = queue.size();
            n = n - size;
            for (int i = 0; i < size; i++) {
                UndirectedGraph cur = queue.poll();
                UndirectedGraph nei = cur.neighbours.get(0);
                nei.neighbours.remove(cur);
                // remmeber to put leaves into next round
                if (nei.neighbours.size() == 1) {
                    queue.offer(nei);
                }
            }
        }

        List<Integer> res = new ArrayList<>();
        for (UndirectedGraph node : queue) {
            res.add(node.val);
        }
        return res;
    } 
}

class UndirectedGraph {
    int val;
    List<UndirectedGraph> neighbours = new ArrayList<>();
    public UndirectedGraph(int value) {
        val = value;
    }
}
```
