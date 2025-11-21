# Detect Cycle in undirected graph

如题，解法跟有向图有点类似，也是用set。主要不同是，这里用一个set就ok了。每次递归时把parent也传进去，如果发现有邻居已经被visit了而且不是parent的时候，我们就知道图里有另一条路走到那个点，所以返回true。S:O（V）T: O(E +V)

```java
public boolean isCyclic(List<UndirectedGraphNode> graph) {
    if (graph == null || graph.size() == 0) {
        return false;
    }

    HashSet<UndirectedGraphNode> visited = new HashSet<>();
    for (UndirectedGraphNode node : graph) {
        if (visited.contains(node)) {
            continue;
        }

        visited.add(node);
        if (dfs(graph, node, null, visited)) {
            return true;
        }
    }

    return false;
}

private boolean dfs(List<UndirectedGraphNode> graph, UndirectedGraphNode current, 
                    UndirectedGraphNode parent, HashSet<UndirectedGraphNode> visited) {

    for (UndirectedGraphNode nei : current.neighbors) {
        if (nei.equals(parent)) {
            continue;
        }

        if (visited.contains(nei) ) {
            return true;
        }

        visited.add(nei);
        if (dfs(graph, nei, current, visited)) {
            return true;
        }
    }

    return false;
}

class UndirectedGraphNode {
    int label;
    List<UndirectedGraphNode> neighbors;
    UndirectedGraphNode(int x) { 
        label = x;
        neighbors = new ArrayList<UndirectedGraphNode>(); 
    }
}
```
