# 256 Paint House

There are a row ofnhouses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a`nx3`cost matrix. For example,`costs[0][0]`is the cost of painting house 0 with color red;`costs[1][2]`is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

**Note:**\
All costs are positive integers.

这里主要注意下标控制。

```java
public int minCost(int[][] costs) {
    if (costs == null || costs.length == 0 || costs[0].length == 0) {
        return 0;
    }

    int n = costs.length;
    int[][] dp = new int[n][3];
    dp[0][0] = costs[0][0];
    dp[0][1] = costs[0][1];
    dp[0][2] = costs[0][2];

    for (int i = 1; i < n; i++) {
        for (int j = 0; j < 3; j++) {
            dp[i][j] = Math.min(dp[i - 1][(j + 1) % 3], dp[i - 1][(j + 2) % 3]) + costs[i][j];
        }
    }

    int min = Integer.MAX_VALUE;
    for (int i = 0; i < 3; i++) {
        min = Math.min(min, dp[n - 1][i]);
    }
    return min;
}

// 直观版
public int minCost(int[][] costs) {
    if (costs == null || costs.length == 0 || costs[0].length == 0) {
        return 0;
    }

    int n = costs.length;
    int m = costs[0].length;
    int[][] dp = new int[n][m];

    for (int j = 0; j < m; j++) {
        dp[0][j] = costs[0][j];
    }

    for (int i = 1; i < n; i++) {
        dp[i][0] = costs[i][0] + Math.min(dp[i - 1][1], dp[i - 1][2]);
        dp[i][1] = costs[i][1] + Math.min(dp[i - 1][0], dp[i - 1][2]);
        dp[i][2] = costs[i][2] + Math.min(dp[i - 1][0], dp[i - 1][1]);            
    }

    int min = Integer.MAX_VALUE;
    for (int j = 0; j < m; j++) {
        min = Math.min(min, dp[n - 1][j]);
    }

    return min;
}
```
