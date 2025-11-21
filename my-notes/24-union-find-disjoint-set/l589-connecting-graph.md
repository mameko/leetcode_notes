# L589 Connecting Graph

Given`n`nodes in a graph labeled from`1`to`n`. There is no edges in the graph at beginning.

You need to support the following method:\
1.`connect(a, b)`, add an edge to connect node`a`and node b`. 2.`query(a, b)\`, check if two nodes are connected

**Example**

```
5 // n = 5
query(1, 2) return false
connect(1, 2)
query(1, 3) return false
connect(2, 4)
query(1, 4) return true
```

这题是基本操作，联通union（connect）和find（query）

```java
public class ConnectingGraph { 

    HashMap<Integer, DSNode> map = new HashMap<>();

    public ConnectingGraph(int n) {
        for (int i = 1; i <= n; i++) {
            makeNode(i);
        }
    }

    public void connect(int a, int b) {
        if (query(a, b)) {// already connected
            return;
        }

        DSNode na = findGroup(map.get(a));
        DSNode nb = findGroup(map.get(b));

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

    public boolean  query(int a, int b) {
        if (a == b) {
            return true;
        }

        DSNode na = findGroup(map.get(a));
        DSNode nb = findGroup(map.get(b));

        return na == nb;
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
