# L272 Climbing Stairs II

A child is running up a staircase with`n`steps, and can hop either 1 step, 2 steps, or 3 steps at a time. Implement a method to count how many possible ways the child can run up the stairs.

**Example**

n=`3`\
1+1+1=2+1=1+2=3=3

return`4`

还是斐布拉其。

```java
public int climbStairs2(int n) {
    if (n <= 0) {
        return 1;
    } else if (n == 1) {
        return 1;
    } else if (n == 2) {
        return 2;
    } else if (n == 3) {
        return 4;
    }

    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    dp[2] = 2;
    dp[3] = 4;
    for (int i = 4; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2] + dp[i - 3];
    }
    return dp[n];
}
```
