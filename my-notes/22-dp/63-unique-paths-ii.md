# 63 Unique Paths II

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as`1`and`0`respectively in the grid.

## Notice

mandnwill be at most 100.

**Example**

For example,

There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```

The total number of unique paths is`2`.

这题跟上一题[62](62-unique-paths.md)的不同在于这里有obstacle。除了obstacle的位置要设成0以为，跟上一题一样。

```java
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
    if (obstacleGrid == null || obstacleGrid.length == 0 || obstacleGrid[0].length == 0 ) {
        return 0;
    }

    int[][] dp = new int[obstacleGrid.length][obstacleGrid[0].length];
    //init 1st col
    for (int i = 0; i < obstacleGrid.length; i++) {
        if (obstacleGrid[i][0] == 1 || (i > 0 && dp[i - 1][0] == 0)) {
            dp[i][0] = 0;
        } else {
            dp[i][0] = 1;
        }
    }

    //init 1st row
    for (int j = 0; j < obstacleGrid[0].length; j++) {
        if (obstacleGrid[0][j] == 1 || (j > 0 && dp[0][j - 1] == 0)) {
            dp[0][j] = 0;
        } else {
            dp[0][j] = 1;
        }
    }

    //fill the dp matrix
    for (int i = 1; i < obstacleGrid.length; i++) {
        for (int j = 1; j < obstacleGrid[0].length; j++) {
            if (obstacleGrid[i][j] == 1) {
                dp[i][j] = 0;
            } else {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
    }

    return dp[obstacleGrid.length - 1][obstacleGrid[0].length - 1];
}
```
