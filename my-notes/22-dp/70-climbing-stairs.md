# 70 Climbing Stairs

You are climbing a stair case. It takes **n** steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Example**

Given an example n=3 , 1+1+1=2+1=1+2=3

return 3

斐布拉其数列。

```java
public int climbStairs(int n) {
    if (n < 1) {
        return 1;
    } else if (n == 1) {
        return 1;
    }

    int[] dp = new int[n + 1];

    dp[1] = 1;
    dp[2] = 2;

    for (int i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
}
```
