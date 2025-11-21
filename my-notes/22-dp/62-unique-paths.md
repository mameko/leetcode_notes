# 62 Unique Paths

A robot is located at the top-left corner of amxngrid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

## Notice

mandnwill be at most 100.

**Example**

| 1,1 | 1,2 | 1,3 | 1,4 | 1,5 | 1,6 | 1,7 |
| --- | --- | --- | --- | --- | --- | --- |
| 2,1 |     |     |     |     |     |     |
| 3,1 |     |     |     |     |     | 3,7 |

Above is a 3 x 7 grid. How many possible unique paths are there?

这题听说可以排列组合一下子算出。不过这里还是用了dp。这里还是填格子解决问题。初始化第一行和第一列为1，因为只能从上面或者左边过来。然后填内容时，因为能从上面或者左边来，所以把两边的个数加起来。最后返回终点时的值。

```java
public int uniquePaths(int m, int n) {
    if(n < 1 || n >100 || m<1 || m>100){
        return -1;
    }

    int[][] dp = new int[m][n];
    for(int i=0;i<m; i++){
        dp[i][0] = 1;
    }

    for(int i=0;i<n; i++){
        dp[0][i] = 1;
    }

    for(int i=1;i<m; i++){
        for(int j=1;j<n; j++){
            dp[i][j] = dp[i][j-1] + dp[i-1][j];
        }
    }

    return dp[m-1][n-1];
}
```
