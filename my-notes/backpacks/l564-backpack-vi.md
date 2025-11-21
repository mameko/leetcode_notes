# L564 Backpack VI

Given an integer array`nums`with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer`target`.

## Notice

The different sequences are counted as different combinations.

**Example**

Given nums =`[1, 2, 4]`, target =`4`

```
The possible combination ways are:
[1, 1, 1, 1]
[1, 1, 2]
[1, 2, 1]
[2, 1, 1]
[2, 2]
[4]
```

return`6`

这题跟前面的那些背包有些不一样。也可以用记忆化搜索来做。

求以1开头的，用【1，2，4】组成 4 - 1 = 3的方法数目

求以2开头的，用【1，2，4】组成 4 - 2 = 2的方法数目

求以4开头的，用【1，2，4】组成 4 - 4 = 0的方法数目

用dfs求解的时候，我们会发现有重复子问题。

eg. dfs(4) = dfs(4 -1) + dfs(4 - 2) + dfs(4 - 4)

\= **dfs(3)** + dfs(2) + dfs(0)

\= **dfs(3 - 1) + dfs(3 - 2)** + dfs(2) + dfs(0)

\= **dfs(2)** + dfs(1) + dfs(2) + dfs(0)

```java
public int backPackVI(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int[] dp = new int[target + 1];
    dp[0] = 1;

    for (int i = 1; i <= target; i++) {
        for (int j = 0; j < nums.length; j++) {
            if (i >= nums[j]) {
                dp[i] = dp[i] + dp[i - nums[j]];
            }
        }
    }

    return dp[target];
}
```
