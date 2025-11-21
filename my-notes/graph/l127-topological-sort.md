# L127 Topological Sort

Given an directed graph, a topological order of the graph nodes is defined as follow:

* For each directed edge`A -> B`in graph, A must before B in the order list.
* The first node in the order can be any node in the graph with no nodes direct to it.

Find any topological order for the given graph.

## Notice

You can assume that there is at least one topological order in the graph.

**Clarification**

[Learn more about representation of graphs](http://www.lintcode.com/help/graph)

**Example**

For graph as follow:

![picture](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcThE9AgZZszyhwe0o9qpp3VyizdIj9kWwMY50HiQEysXvkSLsoZ)

The topological order can be:

```
[0, 1, 2, 3, 4, 5]
[0, 2, 3, 1, 5, 4]
...
```

[**Challenge**](http://www.lintcode.com/en/problem/topological-sorting/#challenge)

Can you do it in both BFS and DFS?

拓扑排序...用bfs

```java
public ArrayList<DirectedGraphNode> topSort(ArrayList<DirectedGraphNode> graph) {
    ArrayList<DirectedGraphNode> res = new ArrayList<>();
    if (graph == null || graph.size() == 0) {
        return res;
    }

    // collect in-degree
    HashMap<DirectedGraphNode, Integer> inDegree = new HashMap<>();
    for (DirectedGraphNode node : graph) {
        for (DirectedGraphNode nei : node.neighbors) {
            if (inDegree.containsKey(nei)) {
                inDegree.put(nei, inDegree.get(nei) + 1);
            } else {
                inDegree.put(nei, 1);
            }
        }
    }

    // put all indegree == 0 point in the result, also put in queue for tranvesal
    Queue<DirectedGraphNode> queue = new LinkedList<>();
    for (DirectedGraphNode node : graph) {
        if (!inDegree.containsKey(node)) {
            res.add(node);
            queue.offer(node);
        }
    }

    // do bfs and add ndoe to result
    while (!queue.isEmpty()) {
        DirectedGraphNode cur = queue.poll();

        for (DirectedGraphNode nei : cur.neighbors) {
            inDegree.put(nei, inDegree.get(nei) - 1);
            if (inDegree.get(nei) == 0) {
                inDegree.remove(nei);
                queue.offer(nei);
                res.add(nei);
            }
        }
    }

    if (inDegree.size() != 0) {
        // don't have topo order
    }

    return res;
}
```
