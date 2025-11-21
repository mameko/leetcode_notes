# L584 Drop Eggs II

There is a building of`n`floors. If an egg drops from the`k`th floor or above, it will break. If it's dropped from any floor below, it will not break.

You're given`m`eggs, Find k while minimize the number of drops for the worst case. Return the number of drops in the worst case.

**Example**

Given`m`=`2`,`n`=`100`return`14`\
Given`m`=`2`,`n`=`36`return`8`

没懂...

```java
public int dropEggs2(int m, int n) {
    if (m <= 0 || n <= 0) {
        return 0;
    }

    int[][] dp = new int[m + 1][n + 1];


    // init first column
    for (int row = 0; row <= m; row++) {
        dp[row][0] = 0;
        dp[row][1] = 1;
    }

        // init first row
    for (int col = 1; col <= n; col++) {
        dp[1][col] = col;
    }

    for (int row = 2; row <= m; row++) {
        for (int col = 2; col <= n; col++) {
            dp[row][col] = Integer.MAX_VALUE;
            for (int k = 1; k <= col; k++) {
                int tmp = 1 + Math.max(dp[row - 1][k - 1], dp[row][col - k]);
                    dp[row][col] = Math.min(tmp, dp[row][col]);
            }
        }
    }
    return dp[m][n];
}
```
