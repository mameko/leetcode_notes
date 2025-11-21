# L591 Connecting Graph III

Given`n`nodes in a graph labeled from`1`to`n`. There is no edges in the graph at beginning.

You need to support the following method:\
1.`connect(a, b)`, an edge to connect node a and node b\
2.`query()`, Returns the number of connected component in the graph

**Example**

```
5 // n = 5
query() return 5
connect(1, 2)
query() return 4
connect(2, 4)
query() return 3
connect(1, 4)
query() return 3
```

这题求图里联通块的个数，用一个全局的变量记录一开始有多少个，每次成功联通时减一。边连边track。

```java
public class ConnectingGraph3 {

    HashMap<Integer, DSNode> map = new HashMap<>();
    int totalNumOfNodes = 0;

    public ConnectingGraph3(int n) {
        for (int i = 1; i <= n; i++) {
            makeNode(i);
        }
        totalNumOfNodes = n;
    }

    public void connect(int a, int b) {
        DSNode na = findGroup(map.get(a));
        DSNode nb = findGroup(map.get(b));

        if (na == nb) {// already connected;
            return;
        }

        if (na.rank >= nb.rank) {// put nodeB under nodeA
            nb.parent = na;
            na.rank += nb.rank;
            nb.rank = -1;
        } else {// put node A under node B
            na.parent = nb;
            nb.rank += na.rank;
            na.rank = -1;
        }
        totalNumOfNodes--;
    }

    public int query() {
        return totalNumOfNodes;
    }

    private DSNode findGroup(DSNode n) {
        DSNode parent = n.parent;
        if (parent == n) {// root
            return n;
        }

        n.parent = findGroup(n.parent);// compress path

        return n.parent;
    }

    private void makeNode(int nodeValue) {
        DSNode newNode = new DSNode(nodeValue);
        newNode.rank = 1;
        newNode.parent = newNode;
        map.put(nodeValue, newNode);
    }
}

class DSNode {
    int val;
    int rank;// node number
    DSNode parent;

    public DSNode(int v) {
        val = v;
    }
}
```
