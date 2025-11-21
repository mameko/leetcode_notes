# 305 Number of Island II

A 2d grid map of`m`rows and`n`columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate,**count the number of islands after each addLand operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**

Given`m = 3, n = 3`,`positions = [[0,0], [0,1], [1,2], [2,1]]`.\
Initially, the 2d grid`grid`is filled with water. (Assume 0 represents water and 1 represents land).

```
0 0 0
0 0 0
0 0 0
```

Operation #1: addLand(0, 0) turns the water at grid\[0]\[0] into a land.

```
1 0 0
0 0 0   Number of islands = 1
0 0 0
```

Operation #2: addLand(0, 1) turns the water at grid\[0]\[1] into a land.

```
1 1 0
0 0 0   Number of islands = 1
0 0 0
```

Operation #3: addLand(1, 2) turns the water at grid\[1]\[2] into a land.

```
1 1 0
0 0 1   Number of islands = 2
0 0 0
```

Operation #4: addLand(2, 1) turns the water at grid\[2]\[1] into a land.

```
1 1 0
0 0 1   Number of islands = 3
0 1 0
```

We return the result as an array:`[1, 1, 2, 3]`

**Challenge:**

Can you do it in time complexity O(k log mn), where k is the length of the`positions`?

基本就是go through每个operation，边加边连，连成了就把数目减一。

```java
    int N;
    int M;
    HashMap<Integer, DSNode> map = new HashMap<>();
    int[] x = { 1, -1, 0, 0 };
    int[] y = { 0, 0, 1, -1 };

    public List<Integer> numIslands2(int n, int m, Point[] operators) {
        List<Integer> res = new ArrayList<>();
        if (n <= 0 || m <= 0 || operators == null || operators.length == 0) {
            return res;
        }
        M = m;
        N = n;
        int islandNum = 0;
        for (Point p : operators) {
            makeNode(p);
            islandNum++;
            for (int k = 0; k < 4; k++) { // check if this node can connect to existing neighor island
                int i = p.x + x[k];
                int j = p.y + y[k];
                Point nei = new Point(i, j);
                DSNode neiNode = map.get(convertToLocation(nei));
                if (inBound(i, j, n, m) && neiNode != null) {
                    if (connect(p, nei)) {
                        islandNum--;
                    }
                }
            }

            res.add(islandNum);
        }
        return res;
    }

    private boolean inBound(int i, int j, int n, int m) {
        if (i < 0 || j < 0 || i >= n || j >= m) {
            return false;
        }
        return true;
    }

    private boolean connect(Point a, Point b) {
        DSNode an = map.get(convertToLocation(a));
        DSNode bn = map.get(convertToLocation(b));

        DSNode anp = findSet(an);
        DSNode bnp = findSet(bn);

        if (anp == bnp) {// already connected
            return false;
        }

        if (anp.rank >= bnp.rank) {
            bnp.parent = anp;
            anp.rank += bnp.rank;
            bnp.rank = -1;
        } else {
            anp.parent = bnp;
            bnp.rank += anp.rank;
            anp.rank = -1;
        }
        return true;
    }

    private DSNode findSet(DSNode node) {
        DSNode parent = node.parent;
        if (parent == node) {
            return parent;
        }

        node.parent = findSet(node.parent);
        return node.parent;
    }

    private void makeNode(Point p) {
        int loc = convertToLocation(p);
        DSNode newNode = new DSNode(loc);
        newNode.rank = 1;
        newNode.parent = newNode;
        map.put(loc, newNode);
    }

    private int convertToLocation(Point p) {
        return p.x * M + p.y;
    }


class DSNode {
    int val;
    int rank;
    DSNode parent;

    public DSNode(Integer v) {
        val = v;
    }
}
```
