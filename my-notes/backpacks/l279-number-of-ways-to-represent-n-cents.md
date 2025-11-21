# L279 Number of Ways to Represent N Cents

Given an infinite number of quarters (25 cents), dimes (10 cents), nickels (5 cents) and pennies (1 cent), write code to calculate the number of ways of representing n cents.

**Example**

n =`11`

```
11 = 1 + 1 + 1... + 1
   = 1 + 1 + 1 + 1 + 1 + 1 + 5
   = 1 + 5 + 5
   = 1 + 10
```

这题其实就是背包IV，每个元素无限取。

```java
public int waysNCents(int n) { // backpack IV
    if (n < 0) {
        return 0;
    }

    int[] coins = {1, 5, 10, 25};
    int len = coins.length;
    int[][] dp = new int[len + 1][n + 1];

    for (int i = 1; i <= len; i++) {
        dp[i][0] = 1;
        for (int j = 1; j <= n; j++) {
            dp[i][j] = dp[i - 1][j];
            if (j >= coins[i - 1]) {
                dp[i][j] = dp[i][j] + dp[i][j - coins[i - 1]];
            }
        }
    }
    return dp[len][n];
}
```
