# L42 Maximum Subarray II

Given an array of integers, find two non-overlapping subarrays which have the largest sum.\
The number in each subarray should be contiguous.\
Return the largest sum.

## Notice

The subarray should contain at least one number

**Example**

For given`[1, 3, -1, 2, -1, 2]`, the two subarrays are`[1, 3]`and`[2, -1, 2]`or`[1, 3, -1, 2]`and`[2]`, they both have the largest sum`7`.

[**Challenge**](http://www.lintcode.com/en/problem/maximum-subarray-ii/#challenge)

Can you do it in time complexity O(_n_) ?

这题左边来一次，右边来一次，最后因为两个subarray不能重叠，所以最后的loop要错开一个位置

```java
public int maxTwoSubArrays(ArrayList<Integer> nums) {
    if (nums == null || nums.size() == 0) {
        return 0;
    }

    int n = nums.size();
    int[] left = new int[n];
    int[] right = new int[n];

    // from left to right find max
    int minSum = 0;
    int preSum = 0;
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        int cur = nums.get(i);
        preSum += cur;
        max = Math.max(max, preSum - minSum);
        minSum = Math.min(minSum, preSum);
        left[i] = max;
    }

    // from right to left find max
    minSum = 0;
    preSum = 0;
    max = Integer.MIN_VALUE;
    for (int i = n - 1; i >= 0; i--) {
        int cur = nums.get(i);
        preSum += cur;
        max = Math.max(max, preSum - minSum);
        minSum = Math.min(minSum, preSum);
        right[i] = max;
    }

    // find max on the whole array. because non-overlap, so need to add the one next to it
    int res = Integer.MIN_VALUE;
    for (int i = 0; i < n - 1; i++) {
        res = Math.max(res, left[i] + right[i + 1]);
    }

    return res;
}
```
