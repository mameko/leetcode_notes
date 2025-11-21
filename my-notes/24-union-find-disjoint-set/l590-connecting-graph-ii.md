# L590 Connecting Graph II

Given`n`nodes in a graph labeled from`1`to`n`. There is no edges in the graph at beginning.

You need to support the following method:\
1.`connect(a, b)`, an edge to connect node a and node b\
2.`query(a)`, Returns the number of connected component nodes which include node`a`.

**Example**

```
5 // n = 5
query(1) return 1
connect(1, 2)
query(1) return 2
connect(2, 4)
query(1) return 3
connect(1, 4)
query(1) return 3
```

这题要求返回每个联通块里node的个数，就是返回rank。

```java
public class ConnectingGraph2 {

    HashMap<Integer, DSNode> map = new HashMap<>();

    public ConnectingGraph2(int n) {
        for (int i = 1; i <= n; i++) {
            makeNode(i);
        }
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
    }

    public int query(int a) {
        DSNode res = findGroup(map.get(a));
        return res.rank;
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
