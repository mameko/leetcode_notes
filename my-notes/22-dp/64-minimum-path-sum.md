# 64 Minimum Path Sum

Given a **m x n** grid filled with non-negative numbers, find a path from top left to bottom right which **minimizes** the sum of all numbers along its path.

## Notice

You can only move either down or right at any point in time.

这题画个二维格子，首先初始化第一行第一列。因为只能从左或上过来，所以把路径的值一直加起来就好了。然后填内容时，找min（上面，左边）加上现在这个格子的值。算到右下就找到答案了。

```java
public int minPathSum(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int[][] dp = new int[grid.length][grid[0].length];
    dp[0][0] = grid[0][0];

    //init 1st col
    for (int i = 1; i < grid.length; i++) {
        dp[i][0] = dp[i - 1][0] + grid[i][0];
    }

    //init 1st row
    for (int j = 1; j < grid[0].length; j++) {
        dp[0][j] = dp[0][j - 1] + grid[0][j];
    }

    for (int i = 1; i < grid.length; i++) {
         for (int j = 1; j < grid[0].length; j++) {
             dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
         }
    }

    return dp[grid.length - 1][grid[0].length - 1];
}
```
