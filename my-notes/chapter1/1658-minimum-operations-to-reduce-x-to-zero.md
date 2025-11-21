# 1658 Minimum Operations to Reduce X to Zero

You are given an integer array `nums` and an integer `x`. In one operation, you can either remove the leftmost or the rightmost element from the array `nums` and subtract its value from `x`. Note that this **modifies** the array for future operations.

Return _the **minimum number** of operations to reduce_ `x` _to **exactly**_ `0` _if it's possible, otherwise, return_ `-1`.

**Example 1:**

```
Input: nums = [1,1,4,2,3], x = 5
Output: 2
Explanation: The optimal solution is to remove the last two elements to reduce x to zero.
```

**Example 2:**

```
Input: nums = [5,6,7,8,9], x = 4
Output: -1
```

**Example 3:**

```
Input: nums = [3,2,20,1,1,3], x = 10
Output: 5
Explanation: The optimal solution is to remove the last three elements and the first two elements (5 operations in total) to reduce x to zero.
```

**Constraints:**

* `1 <= nums.length <= 105`
* `1 <= nums[i] <= 104`
* `1 <= x <= 109`

这题，一开始的思路走错了，本来打算是左边去一个，和右边去一个，然后递归下去，每下一层op + 1。最后再用memo优化，但是，没写出来。后来看了答案，其实是可以用逆向思维做这题。我们可以求[325 Maximum Size Subarray Sum Equals k](../intervals/435-non-overlapping-intervals.md).而k是等于total-x。如果中间那一节的最长子序列等于total - x的话，我们就可以知道在两头到底去掉多少个会等于x了。

但是呢...各种写不对，最后这个twoptr还是抄答案的。其实还有一点要注意的是，我们可能加了整个数组都不够target大，这时候就是无解，要返回-1.因为这题的输入都是正数，所有没有325那么复杂。

```java
public int minOperations(int[] nums, int x) {
    if (nums == null || nums.length == 0 || x < 0) {
        return -1;
    }
    
    int total = 0;
    for (int num : nums) {
        total += num;
    }

    int target = total - x;
    int n = nums.length;
    int maxLen = -1;
    int sumSorFar = 0;
    int left = 0;
    for (int right = 0; right < n; right++) {
        sumSorFar += nums[right];
        while (sumSorFar > target && left <= right) {
            sumSorFar -= nums[left++];
        }

        if (sumSorFar == target) {
            maxLen = Math.max(maxLen, right - left + 1);
        }
    }

    return maxLen == -1 ? -1 : n - maxLen;        
}
```
