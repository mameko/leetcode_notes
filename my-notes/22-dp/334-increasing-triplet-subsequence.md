# 334 Increasing Triplet Subsequence

Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:

> Return true if there exists i, j, k\
> such that arr\[i] < arr\[j] < arr\[k] given 0 ≤ i < j < k ≤ n -1 else return false.

Your algorithm should run in O(n) time complexity and O(1) space complexity.

**Examples:**\
Given`[1, 2, 3, 4, 5]`,\
return`true`.

Given`[5, 4, 3, 2, 1]`,\
return`false`.

感觉这题像[414](../quick-sort/414-third-maximum-number.md)那样，很容易想复杂了，因为这题看上去很像[300](397-longest-increasing-continuous-subsequence.md)，所以一上来我就O(n^2)地写了一个。然后看了答案才发现，原来还有更简单的方法。用两个变量来记录最小，和次小，然后一边loop一边更新，如果找到第三个大于前两个的话，我们就返回true。

O(n)的：

```java
public boolean increasingTriplet(int[] nums) {
    if (nums == null || nums.length < 3) {
        return false;
    }

    int small = Integer.MAX_VALUE;
    int big = Integer.MAX_VALUE;
    for (int elem : nums) {
        if (elem <= small) { //最小
            small = elem;
        } else if (elem <= big) {//次小
            big = elem;
        } else {
            return true;
        }
    }

    return false;
}
```

n方的：

```java
public boolean increasingTriplet(int[] nums) {
    if (nums == null || nums.length < 3) {
        return false;
    }

    int n = nums.length;
    int[] dp = new int[n];

    for (int i = 0; i < n; i++) {
        dp[i] = 1;
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[j] + 1, dp[i]);
                if (dp[i] >= 3) {
                    return true;
                }
            }
        }
    }

    return false;
}
```
