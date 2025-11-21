# 221 Maximal Square

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing all 1's and return its area.

**Example**

For example, given the following matrix:

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```

Return`4`.

本来这题得拆开3个正方形来dp的，不过因为是正方形而且要求全是1，我们可以用1个dp矩阵来解决问题（就算一边的边长）。3个dp的请看L631 Maximal Square II的解法。这里的转移方程的理解是：因为我们是正方形，dp\[i]\[j]记录的是，以ij为右下角的最大正方形。dp右上的方形里最多能延伸到多远（1的个数），dp\[i]\[j-1]记录的是往上走，能到多远，dp\[i - 1]\[j]是记录往左走，能走多远。然后，这个正方形有多大，决定于它们中最短的边。

```java
public int maxSquare(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return 0;
    }

    int max = 0;
    int n = matrix.length;
    int m = matrix[0].length;
    int[][] dp = new int[n][m];
    // init first col
    for (int i = 0; i < n; i++) {
        dp[i][0] = matrix[i][0];
        max = Math.max(max, dp[i][0]);// for case ["1"]
    }

    // init first row
    for (int j = 0; j < m; j++) {
        dp[0][j] = matrix[0][j];
        max = Math.max(max, dp[0][j]);// for case ["1"]
    }


    // cal the rest of dp matrix
    for (int i = 1; i < n; i++) {
        for (int j = 1; j < m; j++) {
            if (matrix[i][j] > 0) {
                dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                max = Math.max(max, dp[i][j]);
            } else {
                dp[i][j] = 0;
            }
        }
    }

    return max * max;
}
```
