# 994 Rotting Oranges

You are given an `m x n` `grid` where each cell can have one of three values:

* `0` representing an empty cell,
* `1` representing a fresh orange, or
* `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

<pre><code><strong>Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
</strong><strong>Output: 4
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
</strong><strong>Output: -1
</strong><strong>Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: grid = [[0,2]]
</strong><strong>Output: 0
</strong><strong>Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.
</strong></code></pre>

&#x20;

**Constraints:**

* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 10`
* `grid[i][j]` is `0`, `1`, or `2`.

这题跟僵尸[L598 Zombie in matrix](l598-zombie-in-matrix.md)那题很像，把所有烂橘子先进队 。然后数日子，最后返回能否把所有橘子都感染了。这里其实还可以省loop。就是我们之前入队的时候可以用一个变量数好的橘子个数。然后bfs的时候，rott一个，减一个。这样最后就不用再两个循环判断是否返回-1了。T:O(n \* m) S: O(n \* m)队列size

另外下面是solution里T:O(n^2 \* m^2) S:O(1)的方法。这个做法省了queue。

![](<../../.gitbook/assets/image (24).png>)

每次都把一层橘子需要被rot的时间加上。最后判断是否还有橘子是新鲜的，=1的。因为每天一minute都得整个grid走一遍，所以多了n\*m的时间。

```java
public int orangesRotting(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return -1;
    }

    int minutes = bfs(grid);        
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == 1) {
                return -1;
            }
        }
    }
    return minutes;
}

int[] x = {0, 0, 1, -1};
int[] y = {1, -1, 0, 0};

private int bfs(int[][] grid) {
    int rowLen = grid.length;
    int colLen = grid[0].length;

    Queue<int[]> queue = new LinkedList<>();
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == 2) {
                queue.offer(new int[] { i, j });
            }
        }
    }

    if (queue.size() == 0) { // 跳过[[0]]这种情况
        return 0;
    }

    int minute = 0;
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] cur = queue.poll();
            
            for (int k = 0; k < 4; k++) {
                int nexti = cur[0] + x[k];
                int nextj = cur[1] + y[k];

                if (inbound(nexti, nextj, rowLen, colLen) && grid[nexti][nextj] == 1) {
                    queue.offer(new int[] { nexti, nextj });
                    grid[nexti][nextj] = 2;
                }
            }
        }
        minute++;
    }

    return minute - 1;
}

private boolean inbound(int i, int j, int row, int col) {
    if (i < 0 || j < 0 || i >= row || j >= col) {
        return false;
    }

    return true;
}

// solution的另一种解法
// run the rotting process, by marking the rotten oranges with the timestamp
public boolean runRottingProcess(int timestamp, int[][] grid, int ROWS, int COLS) {
    int[][] directions = { {-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    // flag to indicate if the rotting process should be continued
    boolean toBeContinued = false;
    for (int row = 0; row < ROWS; ++row)
        for (int col = 0; col < COLS; ++col)
            if (grid[row][col] == timestamp)
                // current contaminated cell
                for (int[] d : directions) {
                    int nRow = row + d[0], nCol = col + d[1];
                    if (nRow >= 0 && nRow < ROWS && nCol >= 0 && nCol < COLS)
                        if (grid[nRow][nCol] == 1) {
                            // this fresh orange would be contaminated next
                            grid[nRow][nCol] = timestamp + 1;
                            toBeContinued = true;
                        }
                }
    return toBeContinued;
}

public int orangesRotting(int[][] grid) {
    int ROWS = grid.length, COLS = grid[0].length;
    int timestamp = 2;
    while (runRottingProcess(timestamp, grid, ROWS, COLS))
        timestamp++;

    // end of process, to check if there are still fresh oranges left
    for (int[] row : grid)
        for (int cell : row)
            // still got a fresh orange left
            if (cell == 1)
                return -1;


    // return elapsed minutes if no fresh orange left
    return timestamp - 2;
}
```
