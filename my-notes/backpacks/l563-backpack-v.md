# L563 Backpack V

Given n items with size`nums[i]`which an integer array and all positive numbers. An integer`target`denotes the size of a backpack. Find the number of possible fill the backpack.

`Each item may only be used once`

**Example**

Given candidate items`[1,2,3,3,7]`and target`7`,

```
A solution set is: 
[7]
[1, 3, 3]
```

return`2`

每个元素有限，跟背包II比较像，max换成+。

```java
public int backPackV(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int n = nums.length;
    int[][] dp = new int[n + 1][target + 1];

    for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= target; j++) {
                dp[i][j] = dp[i - 1][j];// not taking j
                if (j >= nums[i - 1]) {
                    if (j == nums[i - 1]) {
                        dp[i][j] = dp[i][j] + 1;
                    }
                    dp[i][j] = dp[i][j] + dp[i - 1][j - nums[i - 1]];
                }
            }
        }

    return dp[n][target];
}
```
