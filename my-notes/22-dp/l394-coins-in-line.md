# L394 Coins in Line

There are n coins in a line. Two players take turns to take one or two coins from right side until there are no more coins left. The player who take the last coin wins.

Could you please decide the **first** play will win or lose?

**Example**

n =`1`, return`true`.

n =`2`, return`true`.

n =`3`, return`false`.

n =`4`, return`true`.

n =`5`, return`true`.

[**Challenge**](http://www.lintcode.com/en/problem/coins-in-a-line/#challenge)

O(n) time and O(1) memory

这题有两种想法，一种是看现在player，另一种是看先手。我用的是看现在player赢不赢。因为一次可以取1或2个。所以如果现在的player取1个或2个的时候，对手不能赢的话，我们这个player就能赢了。这个题还有个数学解法，return n%3 != 0; 如果是最多取m个，我们%(m+1)

```java
public boolean firstWillWin(int n) {
    if (n < 1) {
        return false;
    } else if (n < 3) {
        return true;
    }

    boolean[] dp = new boolean[n + 1];
    dp[1] = true;
    dp[2] = true;
    for (int i = 3; i <= n; i++) {
        dp[i] = !dp[i - 1] || !dp[i - 2];
    }

    return dp[n];
}
```
