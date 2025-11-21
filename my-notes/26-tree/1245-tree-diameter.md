# 1245 Tree Diameter

Given an undirected tree, return its diameter: the number of **edges** in a longest path in that tree.

The tree is given as an array of `edges` where `edges[i] = [u, v]` is a bidirectional edge between nodes `u` and `v`.  Each node has labels in the set `{0, 1, ..., edges.length}`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/06/14/1397_example_1.PNG)

```
Input: edges = [[0,1],[0,2]]
Output: 2
Explanation: 
A longest path of the tree is the path 1 - 0 - 2.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/06/14/1397_example_2.PNG)

```
Input: edges = [[0,1],[1,2],[2,3],[1,4],[4,5]]
Output: 4
Explanation: 
A longest path of the tree is the path 3 - 2 - 1 - 4 - 5.
```

**Constraints:**

* `0 <= edges.length < 10^4`
* `edges[i][0] != edges[i][1]`
* `0 <= edges[i][j] <= edges.length`
* The given edges form an undirected tree.

这题，一看，怎么这么眼熟，还以为是[543 Diameter of Binary Tree](543-diameter-of-binary-tree.md)。后来，build图build了半天。本来想找root然后bfs的。后来发现是无向图，不能用入度判断是否是根。后来转向去找叶子，因为最长的路径一定是两个叶子之间的。然后发现，叶子的话要都找一遍才能知道哪两个之间是最长的。想了半天无果。后来看答案了。其实，这题可以从任何一个点出发，先找到离它最远的那个点。因为那个点一定是最长路径中的一端。然后再来一次bfs，找到最长路径的另外一端就ok了。还有，bfs的时候，因为是最远的点，所以最后一个visit的叶子就是了。T:O(n), S:O(n)。看了答案，解法二，用[310 Minimum Height Tree](../graph/310-minimum-height-trees.md)的做法，从叶子开始剥洋葱。然后我们把到中间两个点的距离X2，或者X2 + 1（剩下两个点）。

```java
class Solution {    
    public int treeDiameter(int[][] edges) {
        if (edges == null || edges.length == 0 || edges[0].length == 0) {
            return 0;
        }
        
        int n = edges.length + 1;
        int[] indegree = new int[n];
        GraphNode[] graph = new GraphNode[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new GraphNode(i);
        }
        
        for (int[] edge : edges) {
            int from = edge[0];
            int to = edge[1];          
            
            graph[from].children.add(graph[to]);
            graph[to].children.add(graph[from]);
            indegree[to]++;
            indegree[from]++;
        }        
        
        Set<GraphNode> leaves = new HashSet<>();
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 1) {
                leaves.add(graph[i]);
            }
        }
        
		// [distance, nodeNumber]
		int[] oneNode = bfs(0, graph);
		int[] theOtherNode = bfs(oneNode[1], graph);

		return theOtherNode[0];
    }
    
    	private int[] bfs(int start, GraphNode[] graph) {
		int n = graph.length;
		boolean[] visited = new boolean[n];
		
		Queue<GraphNode> queue = new LinkedList<>();
		queue.offer(graph[start]);
		visited[start] = true;

		int level = 0;
		int lastNode = start;
		int longestDis = 0;
		while (!queue.isEmpty()) {
			int size = queue.size();
			for (int i = 0; i < size; i++) {
				GraphNode cur = queue.poll();

				if (cur.children.size() == 1) {
					lastNode = cur.val;
					longestDis = level;
				} 
                for (GraphNode nei : cur.children) {
                    if (!visited[nei.val]) {
                        queue.offer(nei);
                        visited[nei.val] = true;
                    }
                }
			}
			level++;
		}
		return new int[] {longestDis, lastNode};
	}
}

class GraphNode {
    List<GraphNode> children = new ArrayList<>();
    int val;
    
    public GraphNode(int val) {
        this.val = val;
    }
    
    public String toString() {
        return String.valueOf(val);
    }
}
```
