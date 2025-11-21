# 198 House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Example**

Given`[3, 8, 4]`, return`8`.

[**Challenge**](http://www.lintcode.com/en/problem/house-robber/#challenge)

O(n) time and O(1) memory.

这题因为只能隔着抢，所以我们把前两个初始化。然后从第三个开始看往后怎么抢。dp\[0]表示只有一个房子，所以初始化为第一个房子的价值。dp\[1]表示我们有两个房子可以抢，抢比较大的那个。然后第三个房子，我们可以考虑：1，抢这家i和i-2家的价值大，还是2，抢i - 1那家人的大。取最大值。

```java
public long houseRobber(int[] A) {
    long res = 0;
    if (A == null || A.length == 0) {
        return res;
    } else if (A.length == 1) {
        return (long)A[0];
    } else if (A.length == 2) {
        return (long) Math.max(A[0], A[1]);
    }

    long[] dp = new long[A.length];
    dp[0] = (long)A[0];
    dp[1] = (long)A[1];

    for (int i = 2; i < A.length; i++) {
        dp[i] = Math.max(A[i] + dp[i - 2], dp[i - 1]);

    }

    return dp[A.length - 1];
}
```

再来一种多开一位的写法：

```java
public long houseRobber(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }
    
    int n = A.length;
    long[] dp = new long[n + 1];
    
    dp[0] = 0;
    dp[1] = A[0];
    for (int i = 2; i <= n; i++) {
        dp[i] = Math.max(dp[i - 1], dp[i - 2] + A[i - 1]);
    }
    return dp[n];
}
```

滚动数组优化，因为现在状态只跟前两个状态有关，所以只要存2个状态，让新的覆盖旧的就ok了。(最少可以用2，用7，8，9也行）

```java
public long houseRobber(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }
    
    int n = A.length;
    long[] dp = new long[3];
    
    dp[0] = 0;
    dp[1] = A[0];
    for (int i = 2; i <= n; i++) {
        dp[i % 3] = Math.max(dp[(i - 1) % 3], dp[(i - 2) % 3] + A[i - 1]);
    }
    return dp[n % 3];
}
```
