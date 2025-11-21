# 120 Triangle

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is`11`(i.e.,2+3+5+1= 11).

**Note:**\
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

这题可以从上往下dp，也可以从下往上dp。然后dp时注意下标，因为输入的并不是等腰三角形。

从下往上：

```java
    public int minimumTotal(int[][] triangle) {
    if (triangle == null || triangle.length == 0 || triangle[0].length == 0) {
        return 0;
    }

    int n = triangle.length;
    int[][] dp = new int[n][n];

    // init last row
    for (int j = n - 1; j >= 0; j--) {
        dp[n - 1][j] = triangle[n - 1][j];
    }

    for (int i = n - 2; i >= 0; i--) {
        for (int j = i; j >= 0; j--) {
            dp[i][j] = triangle[i][j] + Math.min(dp[i + 1][j], dp[i + 1][j + 1]);
        }
    }

    return dp[0][0];
}
```

从上往下：

```java
public int minimumTotal(int[][] triangle) {
    if (triangle == null || triangle.length == 0) {
        return -1;
    }
    if (triangle[0] == null || triangle[0].length == 0) {
        return -1;
    }

    // state: f[x][y] = minimum path value from x,y to bottom
    int n = triangle.length;
    int[][] f = new int[n][n];

    // initialize 
    for (int i = 0; i < n; i++) {
        f[n - 1][i] = triangle[n - 1][i];
    }

    // bottom up
    for (int i = n - 2; i >= 0; i--) {
        for (int j = 0; j <= i; j++) {
            f[i][j] = Math.min(f[i + 1][j], f[i + 1][j + 1]) + triangle[i][j];
        }
    }

    // answer
    return f[0][0];
}
```
