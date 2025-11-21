# L440 Backpack III

Give&#x6E;_&#x6E;\_kind of items with size Aiand value Vi(_`each item has an infinite number available`_) and a backpack with size\_m_. What's the maximum value can you put into the backpack?

## Notice

You cannot divide item into small pieces and the total size of items you choose should smaller or equal to m.

**Example**

Given 4 items with size`[2, 3, 5, 7]`and value`[1, 5, 2, 4]`, and a backpack with size`10`. The maximum value is`15`.

每件物品可以去多件。所以取这行dp\[i]的来算。

例子：

| size | val |       | 0 | 1 | 2 | 3 | 4 | 5 | 6  | 7  | 8  | 9  | 10 |
| ---- | --- | ----- | - | - | - | - | - | - | -- | -- | -- | -- | -- |
|      |     | **0** | 0 | 0 | 0 | 0 | 0 | 0 | 0  | 0  | 0  | 0  | 0  |
| 2    | 1   | **1** | 0 | 0 | 1 | 0 | 2 | 0 | 3  | 0  | 4  | 0  | 5  |
| 3    | 5   | **2** | 0 | 0 | 1 | 5 | 5 | 6 | 10 | 10 | 11 | 15 | 15 |
| 5    | 2   | **3** | 0 | 0 | 1 | 5 | 5 | 6 | 10 | 10 | 11 | 15 | 15 |
| 7    | 4   | **4** | 0 | 0 | 1 | 5 | 5 | 6 | 10 | 10 | 11 | 15 | 15 |

```java
public int backPackIII(int[] A, int[] V, int m) {
    if (m < 0 || A.length == 0 || V.length == 0) {
        return 0;
    }

    int[][] dp = new int[A.length + 1][m + 1];

    for (int i = 1; i <= A.length; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = dp[i - 1][j];
            if ((j >= A[i - 1])) {
                // because we have unlimited amount of each item, so don't need to i - 1
                dp[i][j] = Math.max(dp[i][j], V[i - 1] + dp[i][j - A[i - 1]]);
            }
        }
    }

    return dp[A.length][m];
}
```
