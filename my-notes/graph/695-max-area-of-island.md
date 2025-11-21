# 695 Max Area of Island

Given a non-empty 2D array`grid`of 0's and 1's, an**island**is a group of`1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

**Example 1:**

```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```

Given the above grid, return`6`. Note the answer is not 11, because the island must be connected 4-directionally.

**Example 2:**

```
[[0,0,0,0,0,0,0,0]]
```

Given the above grid, return`0`.

**Note:**&#x54;he length of each dimension in the given`grid`does not exceed 50.

一开始看还以为有什么陷阱，但好像就真的是跟200 Number of Islands一个套路。但不知道为什么只分在DFS tag里。先写了一个BFS

```java
public int maxAreaOfIsland(int[][] grid) {        
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int n = grid.length;
    int m = grid[0].length;

    boolean[][] visited = new boolean[n][m];        
    int max = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                int cnt = bfs(grid, visited, i, j);
                max = Math.max(max, cnt);
            }
        }
    }

    return max;
}

int[] x = {0, 0, 1, -1};
int[] y = {1, -1, 0, 0};

private int bfs(int[][] grid, boolean[][] visited, int r, int c) {        
    int n = grid.length;
    int m = grid[0].length;

    Queue<Integer> queue = new LinkedList<>();
    queue.offer(r * m + c);
    visited[r][c] = true;
    int cnt = 0;

    while (!queue.isEmpty()) {
        int cur = queue.poll();
        int curi = cur / m;
        int curj = cur % m;
        cnt++;

        for (int k = 0; k < 4; k++) {
            int ni = curi + x[k];
            int nj = curj + y[k];

            if (ni >= 0 && ni < n && nj >= 0 && nj < m && grid[ni][nj] == 1 && !visited[ni][nj]) {
                queue.offer(ni * m + nj);
                visited[ni][nj] = true;
            }
        }
    }

    return cnt;
}
```

再写一个DFS：

```java
public int maxAreaOfIsland(int[][] grid) {        
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int n = grid.length;
    int m = grid[0].length;

    boolean[][] visited = new boolean[n][m];        
    int max = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                int cnt = dfs(grid, visited, i, j);
                max = Math.max(max, cnt);
            }
        }
    }

    return max;
}

int[] x = {0, 0, 1, -1};
int[] y = {1, -1, 0, 0};

private int dfs(int[][] grid, boolean[][] visited, int r, int c) {                
    int res = 1;
    visited[r][c] = true;

    for (int k = 0; k < 4; k++) {
        int ni = r + x[k];
        int nj = c + y[k];

        if (ni >= 0 && ni < grid.length && nj >= 0 && nj < grid[0].length && grid[ni][nj] == 1 && !visited[ni][nj]) {
            res += dfs(grid, visited, ni, nj);
        }
    }

    return res;
}
```
