# Detect Cycle in directed graph

如题，做法是用3个set，白表示还没visited的，灰表示正在visit的，黑表示已经visit完了的。如果在visit的途中发现有正在被visit的（在graySet里）就表示这个点能通过其他的点到达，所以有环。S:O（V）T: O(E +V)

```java
public boolean isCyclic(List<DirectedGraphNode> graph) {
    if (graph == null || graph.size() == 0) {
        return false;
    }

    HashSet<DirectedGraphNode> whiteS = new HashSet<>();
    HashSet<DirectedGraphNode> grayS = new HashSet<>();
    HashSet<DirectedGraphNode> blackS = new HashSet<>();

    for (DirectedGraphNode node : graph) {
        whiteS.add(node);
    }

    while (whiteS.size() > 0) {
        DirectedGraphNode start = whiteS.iterator().next();
        if (dfs(whiteS, grayS, blackS, start)) {
            return true;
        }
    }        

    return false;
}

private boolean dfs(HashSet<DirectedGraphNode> whiteS, HashSet<DirectedGraphNode> grayS,
        HashSet<DirectedGraphNode> blackS, DirectedGraphNode start) {

    move(start, whiteS, grayS);
    for (DirectedGraphNode nei : start.neighbors) {
        if (blackS.contains(nei)) {
            continue;
        }

        if (grayS.contains(nei)) {
            return true;
        }

        if (dfs(whiteS, grayS, blackS, nei)) {
            return true;
        }            
    }

    move(start, grayS, blackS);        
    return false;
}

private void move(DirectedGraphNode start, HashSet<DirectedGraphNode> from, HashSet<DirectedGraphNode> to) {
    from.remove(start);
    to.add(start);        
}

class DirectedGraphNode {
    int label;
    List<DirectedGraphNode> neighbors;
    DirectedGraphNode(int x) { 
        label = x;
        neighbors = new ArrayList<DirectedGraphNode>(); 
    }  

}
```
