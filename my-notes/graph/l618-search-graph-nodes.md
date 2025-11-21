# L618 Search Graph nodes

Given a`undirected graph`, a`node`and a`target`, return the nearest node to given node which value of it is target, return`NULL`if you can't find.

There is a`mapping`store the nodes' values in the given parameters.

## Notice

It's guaranteed there is only one available solution

**Example**

```
2------3  5
 \     |  | 
  \    |  |
   \   |  |
    \  |  |
      1 --4
Give a node 1, target is 50

there a hash named values which is [3,4,10,50,50], represent:
Value of node 1 is 3
Value of node 2 is 4
Value of node 3 is 10
Value of node 4 is 50
Value of node 5 is 50

Return node 4
```

input的那个values是多余的。找最近的=target的点，所以用bfs。首先把起点放进queue里，然后一层一层往外找，如果找到了就返回。

```java
public UndirectedGraphNode searchNode(ArrayList<UndirectedGraphNode> graph,
                                      Map<UndirectedGraphNode, Integer> values,
                                      UndirectedGraphNode node,
                                      int target) {
    if (graph == null || graph.size() == 0) {
        return null;
    }

    Queue<UndirectedGraphNode> queue = new LinkedList<>();
    HashSet<UndirectedGraphNode> visited = new HashSet<>();

    queue.offer(node);
    visited.add(node);

    while (!queue.isEmpty()) {
        UndirectedGraphNode cur = queue.poll();
        if (values.get(cur) == target) {
            return cur;
        }

        for (UndirectedGraphNode nei : cur.neighbors) {
            if (!visited.contains(nei)) {
                queue.offer(nei);
                visited.add(nei);
            }
        }

    }

    return null;
}
```
