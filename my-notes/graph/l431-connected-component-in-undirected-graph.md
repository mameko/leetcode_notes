# L431 Connected Component in Undirected Graph

## L431 Connected Component in Undirected Graph

Find the number connected component in the undirected graph. Each node in the graph contains a label and a list of its neighbors. (a connected component (or just component) of an undirected graph is a subgraph in which any two vertices are connected to each other by paths, and which is connected to no additional vertices in the supergraph.)

### Notice

Each connected component should sort by label.

**Example**

Given graph:

```
A------B  C
 \     |  | 
  \    |  |
   \   |  |
    \  |  |
      D   E
```

Return`{A,B,D}, {C,E}`. Since there are two connected component which is`{A,B,D}, {C,E}`

dfs解：因为是无向图，所以找到一个点就把从它出发能到达的点都加进tmp里，最后遍历完就加到result里。

```java
public List<List<Integer>> connectedSet(ArrayList<UndirectedGraphNode> nodes) {
    List<List<Integer>> res = new ArrayList<>();

    if (nodes == null || nodes.size() == 0) {
        return res;
    }

    HashSet<UndirectedGraphNode> visited = new HashSet<>();
    for (UndirectedGraphNode n : nodes) {
        if (visited.contains(n)) {
            continue;
        }

        ArrayList<Integer> tmpRes = new ArrayList<>();
        search(n, tmpRes, visited);
        Collections.sort(tmpRes);
        res.add(tmpRes);
    }

    return res;
}

private void search(UndirectedGraphNode node, ArrayList<Integer> tmpRes, HashSet<UndirectedGraphNode> visited) {
    tmpRes.add(node.label);
    visited.add(node);
    for (UndirectedGraphNode n : node.neighbors) {
        if (!visited.contains(n)) {
            search(n, tmpRes, visited);
        }
    }
}
```

## 323. Number of Connected Components in an Undirected Graph

Given`n`nodes labeled from`0`to`n - 1`and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

**Example 1:**

```
     0          3
     |          |
     1 --- 2    4
```

Given`n = 5`and`edges = [[0, 1], [1, 2], [3, 4]]`, return`2`.

**Example 2:**

```
     0           4
     |           |
     1 --- 2 --- 3
```

Given`n = 5`and`edges = [[0, 1], [1, 2], [2, 3], [3, 4]]`, return`1`.

**Note:**\
You can assume that no duplicate edges will appear in`edges`. Since all edges are undirected,`[0, 1]`is the same as`[1, 0]`and thus will not appear together in`edges`.

```java
public int countComponents(int n, int[][] edges) {
    if (edges == null || edges.length == 0 || edges[0].length == 0) {
        return n;
    }

    // <node, adjList>
    ArrayList[] graph = new ArrayList[n];
    for (int i = 0; i < n; i++) {
        graph[i] = new ArrayList<Integer>();
    }

    // turn the edges to adjacent list
    for (int[] edge : edges) {
        graph[edge[0]].add(edge[1]);
        graph[edge[1]].add(edge[0]);
    }

    // do bfs to find component number
    int cnt = 0;
    HashSet<Integer> visited = new HashSet<>();
    for (int i = 0; i < n; i++) {
        if (!visited.contains(i)) {
            bfs(graph, visited, i);
            cnt++;
        }
    }

    return cnt;
}

private void bfs(ArrayList[] graph, HashSet<Integer> visited, int start) {
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    visited.add(start);

    while (!queue.isEmpty()) {
        Integer cur = queue.poll();

        for (Object nei : graph[cur]) {
            if (!visited.contains((Integer)nei)) {
                 visited.add((Integer)nei);
                 queue.offer((Integer)nei);
            }
        }
    }
}
```
