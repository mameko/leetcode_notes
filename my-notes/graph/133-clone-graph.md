# 133 Clone Graph

Clone an undirected graph. Each node in the graph contains a`label`and a list of its`neighbors`.

**OJ's undirected graph serialization:**

Nodes are labeled uniquely.

We use`#`as a separator for each node, and`,`as a separator for node label and each neighbor of the node.

As an example, consider the serialized graph`{0,1,2#1,2#2,2}`.

The graph has a total of three nodes, and therefore contains three parts as separated by`#`.

1. First node is labeled as`0`. Connect node`0`to both nodes`1`and`2`.
2. Second node is labeled as`1`. Connect node`1`to node`2`.
3. Third node is labeled as`2`. Connect node`2`to node`2`(itself), thus forming a self-cycle.

Visually, the graph looks like the following:

```
       1
      / \
     /   \
    0 --- 2
         / \
         \_/
```

九章鼓励分开两个步骤做，先复制node再把链接加上。但这个solution在leetcode上TEL了，所以只能一边复制点，一边复制neighbors。T:O(n)，S:O(n)

```java
public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
    if (node == null) {
        return null;
    }

    HashMap<UndirectedGraphNode, UndirectedGraphNode> map = new HashMap<>();
    Queue<UndirectedGraphNode> q1 = new LinkedList<>();
    Queue<UndirectedGraphNode> q2 = new LinkedList<>();

    UndirectedGraphNode newNode = new UndirectedGraphNode(node.label);
    map.put(node, newNode);
    q1.offer(node);
    q2.offer(newNode);

    while (!q1.isEmpty()) {
        UndirectedGraphNode tmp = q1.poll();
        UndirectedGraphNode tmpN = q2.poll();

        ArrayList<UndirectedGraphNode> neiList = new ArrayList<>();
        for (UndirectedGraphNode n : tmp.neighbors) {
           UndirectedGraphNode nei;
                if (map.containsKey(n)) {
                    nei = map.get(n);
                } else {
                    nei = new UndirectedGraphNode(n.label);
                    map.put(n, nei);
                    q1.offer(n);
                    q2.offer(nei);
                }
            neiList.add(nei);

        }
        tmpN.neighbors = neiList;
    }

    return map.get(node);
}
```

分步骤TEL

```java
public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
    if (node == null) {
        return null;
    }

    HashMap<UndirectedGraphNode, UndirectedGraphNode> nodeMap = new HashMap<>();
    // 1. copy all nodes and put them in map
    Queue<UndirectedGraphNode> queue = new LinkedList<>();
    queue.offer(node);
    while (!queue.isEmpty()) {
        UndirectedGraphNode cur = queue.poll();
        if (!nodeMap.containsKey(cur)) {
            UndirectedGraphNode copy = new UndirectedGraphNode(cur.label);
            nodeMap.put(cur, copy);
        }

        for (UndirectedGraphNode nei : cur.neighbors) {
            if (!nodeMap.containsKey(nei)) {// if not visited
                queue.offer(nei);
            }
        }
    }

    // 2. connect the nodes
    for (Map.Entry<UndirectedGraphNode, UndirectedGraphNode> entry : nodeMap.entrySet()) {
        UndirectedGraphNode curNode = entry.getKey();
        UndirectedGraphNode copyNode = entry.getValue();

        ArrayList<UndirectedGraphNode> tmp = new ArrayList<>();
        for (UndirectedGraphNode nei : curNode.neighbors) {
            tmp.add(nodeMap.get(nei));
        }
        copyNode.neighbors = tmp;
    }

    return nodeMap.get(node);

}
```

又刷：

```java
public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
    if (node == null) {
        return node;
    }

    Queue<UndirectedGraphNode> q1 = new LinkedList<>();
    Queue<UndirectedGraphNode> q2 = new LinkedList<>();
    UndirectedGraphNode copy = new UndirectedGraphNode(node.label);
    q1.offer(node);
    q2.offer(copy);

    Map<UndirectedGraphNode, UndirectedGraphNode> hm = new HashMap<>();
    hm.put(node, copy);

    while (!q1.isEmpty()) {
        UndirectedGraphNode cur1 = q1.poll();
        UndirectedGraphNode cur2 = q2.poll();

        List<UndirectedGraphNode> neiList = new ArrayList<>();
        for (UndirectedGraphNode nei1 : cur1.neighbors) {
            UndirectedGraphNode nei2;
            if (hm.containsKey(nei1)) {
                nei2 = hm.get(nei1);
            } else {
                nei2 = new UndirectedGraphNode(nei1.label);
                hm.put(nei1, nei2);
                q1.offer(nei1);
                q2.offer(nei2);
            }
            neiList.add(nei2);
        }

        cur2.neighbors = neiList;
    }

    return copy;
}
```
