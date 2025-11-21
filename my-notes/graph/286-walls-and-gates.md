# 286 Walls and Gates

You are given am x n2D grid initialized with these three possible values.

1. `-1`- A wall or an obstacle.
2. `0`- A gate.
3. `INF`- Infinity means an empty room. We use the value`2^31- 1 = 2147483647`to represent`INF`as you may assume that the distance to a gate is less than`2147483647`.

Fill each empty room with the distance to itsnearestgate. If it is impossible to reach a gate, it should be filled with`INF`.

For example, given the 2D grid:

```
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```

After running your function, the 2D grid should be:

```
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```

经典bfs。

```java
public void wallsAndGates(int[][] rooms) {
    if(rooms == null || rooms.length == 0 || rooms[0].length == 0) {
        return;
    }

    int n = rooms.length;
    int m = rooms[0].length;

    Queue<Integer> queue = new LinkedList<>();
    // put gates in queue
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (rooms[i][j] == 0) {
                queue.offer(i * m + j);
            }
        }
    }

    int[] x = {0, 0, 1, -1};
    int[] y = {1, -1, 0, 0};

    while (!queue.isEmpty()) {
        int cur = queue.poll();
        int curi = cur / m;
        int curj = cur % m;

        for (int k = 0; k < 4; k++) {
            int ni = curi + x[k];
            int nj = curj + y[k];
            if (inBound(ni, nj, rooms) && rooms[curi][curj] + 1 < rooms[ni][nj]) {
                rooms[ni][nj] = rooms[curi][curj] + 1;
                queue.offer(ni * m + nj);
            } 
        }
    }
}

private boolean inBound(int row, int col, int[][] rooms) {
    if (row < 0 || col < 0 || row == rooms.length || col == rooms[0].length) {
        return false;
    }

    return true;
}
```
