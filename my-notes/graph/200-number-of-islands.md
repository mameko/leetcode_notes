# 200 Number of Islands

Given a 2d grid map of`'1'`s (land) and`'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
11110
11010
11000
00000
```

Answer: 1

**Example 2:**

```
11000
11000
00100
00011
```

Answer: 3

这题可以用bfs或者dfs，也能union find。这里就列前两种，Number of island II用union find会在另一章写。这两种方法的时间复杂度都一样，但空间不太一样，bfs会比较少一点，因为dfs最坏情况是全个matrix都是island，O(n^2)的空间，但bfs只会把下一层的1装到队列里，所以不会有O(n^2)那么大。by the way, union find是必须得O(n^2)的空间，因为要为每一个点建一个disjoint set

方法一BFS：

```java
int[] x = {0, 0, 1, -1};
int[] y = {1, -1, 0, 0};

public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int n = grid.length;
    int m = grid[0].length;

    int res = 0;
    boolean[][] visited = new boolean[n][m];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && grid[i][j] == '1') {
                bfs(visited, i, j, grid);
                res++;
            }
        }
    }

    return res;
}

private void bfs(boolean[][] visited, int row, int col, char[][] grid) {
    int n = grid.length;
    int m = grid[0].length;

    Queue<Integer> queue = new LinkedList<>();
    queue.offer(row * m + col);
    visited[row][col] = true;

    while (!queue.isEmpty()) {
        int cur = queue.poll();
        int i = cur / m;
        int j = cur % m;

        for (int k = 0; k < 4; k++) {
            int nexti = i + x[k];
            int nextj = j + y[k];

            if (inBound(nexti, nextj, grid) && !visited[nexti][nextj] && grid[nexti][nextj] == '1') {
                queue.offer(nexti * m + nextj);
                visited[nexti][nextj] = true;
            }
        }
    }
}

private boolean inBound(int row, int col, char[][] grid) {
    if (row < 0 || col < 0 || row >= grid.length || col >= grid[0].length) {
        return false;
    }

    return true;
}
```

方法二DFS：（Lint上的题目输入有点不一样，不是0/1，是T/F）

```java
int m = 0;
int n = 0;
int sum = 0;
int[] x = {0, 0, 1, -1};
int[] y = {1, -1, 0, 0};

public int numIslands(boolean[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    m = grid.length;
    n = grid[0].length;
    boolean[][] visited = new boolean[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (!visited[i][j] && grid[i][j]) {
                dfs(grid, visited, i, j);
                sum++;
            }
        }
    }
    return sum;
}

private void dfs(boolean[][] m, boolean[][] visited, int i, int j) {
    if (!inBounds(i, j)) {
        return;
    }

    if (m[i][j] && !visited[i][j]) {
        visited[i][j] = true;
        for (int k = 0; k < 4; k++) {
                dfs(m, visited, i + x[k], j + y[k]);
        }
    }
}


private boolean inBounds(int i, int j) {
    if (i < 0 || j < 0 || i >= m || j >= n) {
        return false;
    }
    return true;
}
```
