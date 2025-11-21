# L611 Knight Shortest Path

Given a knight in a chessboard (a binary matrix with`0`as empty and`1`as barrier) with a`source`position, find the shortest path to a`destination`position, return the length of the route.\
Return`-1`if knight can not reached.

## Notice

source and destination must be empty.\
Knight can not enter the barrier.

**Clarification**

If the knight is at (_x_,_y_), he can get to the following positions in one step:

```
(x + 1, y + 2)
(x + 1, y - 2)
(x - 1, y + 2)
(x - 1, y - 2)
(x + 2, y + 1)
(x + 2, y - 1)
(x - 2, y + 1)
(x - 2, y - 1)
```

**Example**

```
[[0,0,0],
 [0,0,0],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return 2

[[0,1,0],
 [0,0,0],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return 6

[[0,1,0],
 [0,0,1],
 [0,0,0]]
source = [2, 0] destination = [2, 2] return -1
```

因为是象棋里面的knight的走法，所以方向矩阵有点不太一样。除此之外跟普通的bfs搜索没太大的区别。用一个visitedAndDis矩阵来记录某个点是否能到达。初始化为MAX表示搜之前假设都不能到达。每次如果找到更短的路径，更新一下距离。

```java
/**
 * Definition for a point.
 * public class Point {
 *     publoc int x, y;
 *     public Point() { x = 0; y = 0; }
 *     public Point(int a, int b) { x = a; y = b; }
 * }
 */
public int shortestPath(boolean[][] grid, Point source, Point destination) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return -1;
    }

    int si = source.x;
    int sj = source.y;

    int di = destination.x;
    int dj = destination.y;

    int n = grid.length;
    int m = grid[0].length;

    // in chess, knight can move these 8 ways.
    int[] x = {-2, -2, -1, -1, 1, 1, 2, 2};
    int[] y = {-1, 1, 2, -2, 2, -2, 1, -1};

    int[][] visitedAndDis = new int[n][m];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            visitedAndDis[i][j] = Integer.MAX_VALUE;
        }
    }

    Queue<Point> queue = new LinkedList<>();
    queue.offer(source);
    visitedAndDis[si][sj] = 0;

    while (!queue.isEmpty()) {
        Point cur = queue.poll();
        int curi = cur.x;
        int curj = cur.y;
        int curDis = visitedAndDis[curi][curj];

        for (int k = 0; k < 8; k++) {
            int nexti = curi + x[k];
            int nextj = curj + y[k];

            if (inBound(nexti, nextj, grid) && !grid[nexti][nextj] &&  curDis + 1 < visitedAndDis[nexti][nextj]) {
                visitedAndDis[nexti][nextj] = curDis + 1;
                queue.offer(new Point(nexti, nextj));
            }
        }
    }

    return visitedAndDis[di][dj] == Integer.MAX_VALUE ? -1 : visitedAndDis[di][dj];
}

private boolean inBound(int row, int col, boolean[][] grid) {
    if (row < 0 || col < 0 || row >= grid.length || col >= grid[0].length) {
        return false;
    }
    return true;
}
```
