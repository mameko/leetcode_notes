# L45 Maximum Subarray Difference

Given an array with integers.

Find tw&#x6F;_&#x6E;on-overlapping\_subarrays\_A\_and\_B_, which`|SUM(A) - SUM(B)|`is the largest.

Return the largest difference.

## Notice

The subarray should contain at least one number

**Example**

For`[1, 2, -3, 1]`, return`6`.

[**Challenge**](http://www.lintcode.com/en/problem/maximum-subarray-difference/#challenge)

O(n) time and O(n) space.

做完前面几题，这题就很straight forward了。最后loop的时候把|A - B|拆开成 A - B或 B - A

```java
public int maxDiffSubArrays(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int n = nums.length;
    int[] leftMax = new int[n];
    int[] leftMin = new int[n];
    int[] rightMax = new int[n];
    int[] rightMin = new int[n];
    int[] negNums = new int[n];

    // make a minus copy of the array
    for (int i = 0; i < n; i++) {
        negNums[i] = nums[i] * -1;
    }

    // find max from left to right
    int max = Integer.MIN_VALUE;
    int preSum = 0;
    int minSum = 0;
    for (int i = 0; i < n; i++) {
        preSum = preSum + nums[i];
        max = Math.max(max, preSum - minSum);
        minSum = Math.min(minSum, preSum);
        leftMax[i] = max;
    }

    // find min form left to right
    max = Integer.MIN_VALUE;
    preSum = 0;
    minSum = 0;
    for (int i = 0; i < n; i++) {
        preSum = preSum + negNums[i];
        max = Math.max(max, preSum - minSum);
        minSum = Math.min(minSum, preSum);
        leftMin[i] = max * -1;
    }

    // find max from right to left
    max = Integer.MIN_VALUE;
    preSum = 0;
    minSum = 0;
    for (int i = n - 1; i >= 0; i--) {
        preSum = preSum + nums[i];
        max = Math.max(max, preSum - minSum);
        minSum = Math.min(minSum, preSum);
        rightMax[i] = max;
    }

    // find min from right to left
    max = Integer.MIN_VALUE;
    preSum = 0;
    minSum = 0;
    for (int i = n - 1; i >= 0; i--) {
        preSum = preSum + negNums[i];
        max = Math.max(max, preSum - minSum);
        minSum = Math.min(minSum, preSum);
        rightMin[i] = max * -1;
    }

    // loop through the results again to find the answer
    int res = Integer.MIN_VALUE;
    for (int i = 0; i < n - 1; i++) {
        res = Math.max(res, Math.abs(leftMax[i] - rightMin[i + 1]));
        res = Math.max(res, Math.abs(leftMin[i] - rightMax[i + 1]));
    }

    return res;
}
```
