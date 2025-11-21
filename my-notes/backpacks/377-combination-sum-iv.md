# 377 Combination Sum IV

Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

**Example:**

```
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```

**Follow up:**\
What if negative numbers are allowed in the given array?\
How does it change the problem?\
What limitation we need to add to the question to allow negative numbers?

这题跟backpackVI是一样的，主要不同是follow up。follow up的解释请参照：[http://xyma.me/2016/12/13/Combination-Sum-IV/](http://xyma.me/2016/12/13/Combination-Sum-IV/)

允许负数或0的话，会导致出现无限多的结果，因为多加一个0 或者一对+a，-a，不会影响和的大小。解决方案只能限制return值的长度。

```java
public int combinationSum4(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int[] dp = new int[target + 1];
    dp[0] = 1;
    for (int i = 1; i <= target; i++) {
        for (int j = 0; j < nums.length; j++) {
            if (i >= nums[j]) {
                dp[i] += dp[i - nums[j]];
            }
        }
    }
    return dp[target];
}
```
