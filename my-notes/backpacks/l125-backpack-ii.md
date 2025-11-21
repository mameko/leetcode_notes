# L125 Backpack II

Give&#x6E;_&#x6E;\_items with size Aiand value Vi, and a backpack with size\_m_. What's the maximum value can you put into the backpack?

## Notice

You cannot divide item into small pieces and the total size of items you choose should smaller or equal to m.

**Example**

Given 4 items with size`[2, 3, 5, 7]`and value`[1, 5, 2, 4]`, and a backpack with size`10`. The maximum value is`9`.

[**Challenge**](http://www.lintcode.com/en/problem/backpack-ii/#challenge)

O(n x m) memory is acceptable, can you do it in O(m) memory?

每个只能取一次。所以取上一行dp\[i - 1]的

例子：

| size | val |       | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| ---- | --- | ----- | - | - | - | - | - | - | - | - | - | - | -- |
|      |     | **0** | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0  |
| 2    | 1   | **1** | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1  |
| 3    | 5   | **2** | 0 | 0 | 1 | 5 | 5 | 6 | 6 | 6 | 6 | 6 | 6  |
| 5    | 2   | **3** | 0 | 0 | 1 | 5 | 5 | 6 | 6 | 6 | 7 | 7 | 8  |
| 7    | 4   | **4** | 0 | 0 | 1 | 5 | 5 | 6 | 6 | 6 | 7 | 7 | 9  |

```java
public int backPackII(int m, int[] A, int V[]) {
    if (m < 0 || A.length == 0 || V.length == 0) {
        return -1;
    }

    int[][] dp = new int[A.length + 1][m + 1];

    for (int i = 1; i <= A.length; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = dp[i - 1][j];
            if ((j >= A[i - 1])) {
                dp[i][j] = Math.max(dp[i][j], V[i - 1] + dp[i - 1][j - A[i - 1]]);//因为物品有限，从dp[i-1]取
            }
        }
    }

    return dp[A.length][m];
}
```
