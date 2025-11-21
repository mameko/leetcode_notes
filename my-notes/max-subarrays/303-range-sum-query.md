# 303 Range Sum Query - immutable

Given an integer array nums, find the sum of the elements between indicesiandj(i≤j), inclusive.

**Example:**

```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

**Note:**

1. You may assume that the array does not change.
2. There are many calls to sumRange function.

这题就是求个一维的prefix sum，然后查询。

```java
int[] preSum;
public NumArray(int[] nums) {
    int n = nums.length;
    preSum = new int[n + 1];

    for (int i = 1; i <= n; i++) {
        preSum[i] = nums[i - 1] + preSum[i - 1];
    }
}

public int sumRange(int i, int j) {
    if (i < 0 || j < 0 || i >= preSum.length || j >= preSum.length) {
        return Integer.MIN_VALUE;
    }
    return preSum[j + 1] - preSum[i];
}
```
