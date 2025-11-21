# L562 Backpack IV

Given n items with size`nums[i]`which an integer array and all positive numbers, no duplicates. An integer`target`denotes the size of a backpack. Find the number of possible fill the backpack.

`Each item may be chosen unlimited number of times`

**Example**

Given candidate items`[2,3,6,7]`and target`7`,

```
A solution set is: 
[7]
[2, 2, 3]
```

return`2`

每个元素无限取，形式跟背包III很像，就把max换成+。

```java
public int backPackIV(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int n = nums.length;
    int[][] dp = new int[n + 1][target + 1];

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= target; j++) {
            dp[i][j] = dp[i - 1][j];// not taking j
            if (j >= nums[i - 1]) {
                // we can init 1st col to 1 instead
                if (j == nums[i - 1]) {
                    dp[i][j] = dp[i][j] + 1;
                }
                dp[i][j] = dp[i][j] + dp[i][j - nums[i - 1]];
            }
        }
    }

    return dp[n][target];
}
```
