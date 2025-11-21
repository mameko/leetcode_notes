# L117 Jump Game II

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

**Example**

Given array A =`[2,3,1,1,4]`

The minimum number of jumps to reach the last index is`2`. (Jump`1`step from index 0 to 1, then`3`steps to the last index.)

跟上题很相似。

```java
public int jump(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }

    int n = A.length;
    int[] dp = new int[n];

    Arrays.fill(dp, Integer.MAX_VALUE);
    dp[0] = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (A[j] + j >= i && dp[j] != Integer.MAX_VALUE) {
                dp[i] = Math.min(dp[j] + 1, dp[i]);
            }
        }
    }

    return dp[n - 1];

}
```

这里把O(n)的贪心也贴上，仅供参考。

```java
public int jump(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }

    int cur = 0;
    int i = 0;
    int res = 0;
    int n = A.length - 1;

    while (cur < n) {
        int pre = cur;
        while (i <= pre) {
            cur = Math.max(cur, A[i] + i);
            i++;
        }
        ++res;
        if (pre == cur) {//cur not updated means can't reach end
            return -1;
        }
    }

    return res;
}
```
