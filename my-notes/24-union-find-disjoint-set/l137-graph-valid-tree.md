# L137 Graph Valid Tree

Given`n`nodes labeled from`0`to`n - 1`and a list of`undirected`edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

## Notice

You can assume that no duplicate edges will appear in edges. Since all edges are`undirected`,`[0, 1]`is the same as`[1, 0]`and thus will not appear together in edges.

**Example**

Given`n = 5`and`edges = [[0, 1], [0, 2], [0, 3], [1, 4]]`, return true.

Given`n = 5`and`edges = [[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]`, return false.

合法树的条件是：1. 全部点都连在一起；2. 不能有环。边连边查是否有环（两个点已经connected就有了），最后还得check一下是否全部连在一齐。

```java
    HashMap<Integer, DSNode> map = new HashMap<>();
    public boolean validTree(int n, int[][] edges) {
        if (n <= 0 || edges == null || (edges.length == 0 && n == 1)) {
            return true;
        }

        for (int i = 0; i < n; i++) {
            makeSet(i);
        }

        for (int[] pair : edges) {
            boolean loopExist = connect(pair[0], pair[1]);
            if (loopExist) {
                return false;
            }
        }

        // 看了一下九章的答案，其实可以直接判断，边数==点数-1
        // 如果不等就证明有些点没连上，不用这样判断，囧    
        DSNode lastP = findSet(map.get(0));
        for (int i = 1; i < n; i++) {
            DSNode curP = findSet(map.get(i));
            if (curP != lastP) {
                return false;
            }
        }

        return true;
    }

    private boolean connect(int a, int b) {
        DSNode na = map.get(a);
        DSNode nb = map.get(b);

        DSNode nap = findSet(na);
        DSNode nbp = findSet(nb);

        if (nap == nbp) {
            return true;
        }

        if (nap.rank >= nbp.rank) {
            nbp.parent = nap;
            nap.rank += nbp.rank;
            nbp.rank = -1;
        } else {
            nap.parent = nbp;
            nbp.rank += nap.rank;
            nap.rank = -1;
        }

        return false;
    }

    private DSNode findSet(DSNode node) {
        DSNode parent = node.parent;
        if (parent == node) {
            return parent;
        }

        node.parent = findSet(node.parent);
        return node.parent;
    }

    private void makeSet(int val) {
        DSNode newNode = new DSNode(val);
        newNode.parent = newNode;
        newNode.rank = 1;
        map.put(val, newNode);
    }
}

class DSNode {
    int val;
    int rank;
    DSNode parent;

    public DSNode(int v) {
        val = v;
    }
}
```

```java
// 再写一遍
    Map<Integer, DSNode> map = new HashMap<>();
    public boolean validTree(int n, int[][] edges) {
        if (edges == null) {
            return false;
        }

        if (edges.length != n - 1) {
            return false;
        }

        for (int i = 0; i < n; i++) {
            makeNode(i);
        }

        for (int[] edge : edges) {
            DSNode from = map.get(edge[0]);
            DSNode to = map.get(edge[1]);

            if (!connect(from, to)) {
                return false;
            }
        }

        return true;
    }

    private boolean connect(DSNode n1, DSNode n2) {       
        DSNode root1 = findSet(n1);
        DSNode root2 = findSet(n2);

        if (root1 == root2) {
            return false;
        }

        if (root1.rank > root2.rank) {
            root2.parent = root1;
            root1.rank += root2.rank;
            root2.rank = -1;
        } else {
            root1.parent = root2;
            root2.rank += root1.rank;
            root1.rank = -1;
        }

        return true;
    }

    private DSNode findSet(DSNode cur) {
        DSNode curP = cur.parent;
        if (curP == cur) {
            return cur;
        }

        cur.parent = findSet(cur.parent);
        return cur.parent;
    }

    private void makeNode(int val) {
        DSNode node = new DSNode(val);
        node.parent = node;
        map.put(val, node);
    }
}

class DSNode {
    int val;
    DSNode parent;
    int rank;

    public DSNode(int v) {
        val = v;
        rank = 1;
    }
}
```
