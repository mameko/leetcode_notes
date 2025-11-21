# 416 Partition Equal Subset Sum

Given a**non-empty**array containing**only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Note:**

1. Each of the array element will not exceed 100.
2. The array size will not exceed 200.

**Example 1:**

```
Input: [1, 5, 11, 5]
Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**

```
Input: [1, 2, 3, 5]
Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```

这题其实是变相背包，先看能不能把total number切成2段，如果能，就求是否能背包成total/2.其实就是求有没有combination sum等于total/2

```java
public boolean canPartition(int[] nums) {
    if (nums == null || nums.length == 0) {
        return false;
    }

    int n = nums.length;
    int total = 0;
    for (int i = 0; i < n; i++) {
        total += nums[i];
    }

    if (total % 2 != 0) {
        return false;
    }        

    int target = total / 2;
    boolean[][] dp = new boolean[n + 1][target + 1];

    for (int i = 0; i <= n; i++) {
        dp[i][0] = true;
    }

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= target; j++) {    
            dp[i][j] = dp[i - 1][j];
            if (j >= nums[i - 1] && dp[i - 1][j - nums[i - 1]]) {
                dp[i][j] = true;                                        
            }                                
        }
    }

    return dp[n][target];
}
```
