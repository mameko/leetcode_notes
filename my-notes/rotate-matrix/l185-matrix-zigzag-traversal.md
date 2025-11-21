# L185 Matrix Zigzag Traversal

Given a matrix of\_m\_x\_n\_elements (\_m\_rows,\_n\_columns), return all elements of the matrix in ZigZag-order.

**Example**

Given a matrix:

```
[
  [1, 2,  3,  4],
  [5, 6,  7,  8],
  [9,10, 11, 12]
]
```

return`[1, 2, 5, 9, 6, 3, 4, 7, 10, 11, 8, 12]`

一眼看上去，感觉可以BFS，跟[103 Binary Tree Zigzag Level Order Traversal](../26-tree/103-binary-tree-zigzag-level-order-traversal.md)很像。然后snapchat面经的[对角线打印矩阵](l185-matrix-zigzag-traversal.md)跟[102 Binary Tree Level Order Traversal](../26-tree/102-binary-tree-level-order-traversal.md)有点像。然而好像想复杂了，九章的答案比较简洁，所以也贴上。

```java
public int[] printZMatrix(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return null;
    }

    int n = matrix.length;
    int m = matrix[0].length;

    int[] res = new int[n * m];
    boolean[][] visited = new boolean[n][m];
    Queue<Pair> q = new LinkedList<>();

    q.offer(new Pair(0, 0, matrix[0][0]));
    visited[0][0] = true;
    int ind = 0;
    boolean forward = false;

    while (!q.isEmpty()) {
        int size = q.size();
        ArrayList<Integer> tmp = new ArrayList<>();
        for (int s = 0; s < size; s++) {
            Pair cur = q.poll();
            tmp.add(cur.v);
            // right
            if (inBound(cur.r, cur.c + 1, matrix) && !visited[cur.r][cur.c + 1]) {
                visited[cur.r][cur.c + 1] = true;
                q.offer(new Pair(cur.r, cur.c + 1, matrix[cur.r][cur.c + 1]));
            }

            // down
            if (inBound(cur.r + 1, cur.c, matrix) && !visited[cur.r + 1][cur.c]) {
                visited[cur.r + 1][cur.c] = true;
                q.offer(new Pair(cur.r + 1, cur.c, matrix[cur.r + 1][cur.c]));
            }

        }
        if (forward) {
            forward = false;  
        } else {
            Collections.reverse(tmp);
            forward = true;
        }

        for (Integer elem : tmp) {
            res[ind++] = elem;
        }
    }

    return res;

}

private boolean inBound(int row, int col, int[][] m) {
    if (row < 0 || col < 0 || row >= m.length || col >= m[0].length) {
        return false;
    }

    return true;
}

class Pair {
    int r;
    int c;
    int v;

    public Pair(int row, int col, int val) {
        r = row;
        c = col;
        v = val;
    }
}
```

九章的答案：

```java
public int[] printZMatrix(int[][] matrix) {
    // write your code here
    int x, y, dx, dy, count, m, n;
    x = y = 0;
    count = 1;
    dx = -1; dy = 1;
    m = matrix.length;
    n = matrix[0].length;
    int[] z = new int[m*n];
    z[0] = matrix[0][0];
    while (count<m*n) {
        if (x+dx>=0 && y+dy>=0 && x+dx<m && y+dy<n) {
            x += dx; y += dy;
        }
        else
            if (dx==-1 && dy ==1) {
                if (y+1<n) ++y; else ++x;
                dx = 1; dy = -1;
            }
            else {
                if (x+1<m) ++x; else ++y;
                dx = -1; dy = 1;
            }
        z[count] = matrix[x][y]; ++count;
    }
    return z;
}
```
