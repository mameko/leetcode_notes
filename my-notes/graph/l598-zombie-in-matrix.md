# L598 Zombie in Matrix

Given a 2D grid, each cell is either a wall`2`, a zombie`1`or people`0`(the number zero, one, two).Zombies can turn the nearest people(up/down/left/right) into zombies every day, but can not through wall. How long will it take to turn all people into zombies? Return`-1`if can not turn all people into zombies.

**Example**

Given a matrix:

```
0 1 2 0 0
1 0 0 2 1
0 1 0 0 0
```

return`2`

这题又是很经典的bfs遍历题。首先把所有僵尸都放进queue里，然后一层一层地向邻居感染。每一天看作是一层bfs。最后还得注意一下判断是否所有人都变成僵尸了。day最后减一是因为最后一圈在queue里的僵尸会被放出来，days会++，但这时候其实所有人已经是僵尸了，所以多加了一天。

```java
public int zombie(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int n = grid.length;
    int m = grid[0].length;
    // do bfs, first put all the '1's in the queue
    Queue<Integer> queue = new LinkedList<>();

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                queue.offer(i * m + j);
            }
        }
    }

    int[] x = {0, 0, 1, -1};
    int[] y = {1, -1, 0, 0};
    int days = 0;
    // now we do bfs, and count how much round it needs to finished.
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int s = 0; s < size; s++) {
            int cur = queue.poll();
            int curi = cur / m;
            int curj = cur % m;

            for (int k = 0; k < 4; k++) {
                int nexti = curi + x[k];
                int nextj = curj + y[k];
                if (inBound(nexti, nextj, grid) && grid[nexti][nextj] == 0) { // here use grid != 0 as visited
                    grid[nexti][nextj] = 1;
                    queue.offer(nexti * m + nextj);
                }
            }

        }
        days++;
    }

    // check whether we turn all people into Zombie
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 0) {
                return -1;
            }
        }
    }

    return days - 1;
}

private boolean inBound(int row, int col, int[][] grid) {
    if (row < 0 || col < 0 || row >= grid.length || col >= grid[0].length) {
        return false;
    }
    return true;
}
```
