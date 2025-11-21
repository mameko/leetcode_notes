# L531 Six Degrees

Six degrees of separation is the theory that everyone and everything is six or fewer steps away, by way of introduction, from any other person in the world, so that a chain of "a friend of a friend" statements can be made to connect any two people in a maximum of six steps.

Given a friendship relations, find the degrees of two people, return`-1`if they can not been connected by friends of friends.

**Example**

Gien a graph:

```
1------2-----4
 \          /
  \        /
   \--3--/
```

`{1,2,3#2,1,4#3,1,4#4,2,3}`and s =`1`, t =`4`return`2`

Gien a graph:

```
1      2-----4
             /
           /
          3
```

`{1#2,4#3,4#4,2,3}`and s =`1`, t =`4`return`-1`

这题用bfs，dfs写了但没过，能bfs的都bfs比较好。用一个visted来记录是否已经访问过，并且记录下起始点到这个点之间的距离。首先把起点塞进队列里，然后每层把没访问的邻居放进队列里而且把距离+1，如果找到了target的node就返回step+1（因为这个step是上一个点的距离，要加一才是这个点的距离）

```java
public int sixDegrees(List<UndirectedGraphNode> graph,
                      UndirectedGraphNode s,
                      UndirectedGraphNode t) {

    if (s == t) {
        return 0;
    }

    HashMap<UndirectedGraphNode, Integer> visited = new HashMap<>();
    Queue<UndirectedGraphNode> queue = new LinkedList<>();

    queue.offer(s);
    visited.put(s, 0);
    while (!queue.isEmpty()) {
        UndirectedGraphNode node = queue.poll();
        int step = visited.get(node);
        for (UndirectedGraphNode n : node.neighbors) {
            if (visited.containsKey(n)) {
                continue;
            }

            if (n == t) {
                return  step + 1;
            }

            visited.put(n, step + 1);
            queue.offer(n);
        }
    }
    return -1;
}
```
