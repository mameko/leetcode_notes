# 413 arithmetic Slices

An integer array is called arithmetic if it consists of **at least three elements** and if the difference between any two consecutive elements is the same.

* For example, `[1,3,5,7,9]`, `[7,7,7,7]`, and `[3,-1,-5,-9]` are arithmetic sequences.

Given an integer array `nums`, return _the number of arithmetic **subarrays** of_ `nums`.

A **subarray** is a contiguous subsequence of the array.

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: 3
Explanation: We have 3 arithmetic slices in nums: [1, 2, 3], [2, 3, 4] and [1,2,3,4] itself.
```

**Example 2:**

```
Input: nums = [1]
Output: 0
```

**Constraints:**

* `1 <= nums.length <= 5000`
* `-1000 <= nums[i] <= 1000`

一开始想来想去，觉得这题应该挺像LIS的。但一直显不出来规律。后来看了答案，发现其实brute force也不是很高复杂度，很brute force地一段一段找T:O(N3)或者一发现不能形成arithmetic slice就break掉T:O(N2)。DP的规律是每次增加一个合资格的数字时，我们知道到这个i为止的arithmetic slice等于前一段加1。因为算的是总和，所以sum是把所有的加起来。但我们发现i不能增加arithmetic slice长度时，我们就把值设置成0，等后面又出现arithmetic slice时，从新计数。

```java
public int numberOfArithmeticSlices(int[] A) {
    if (A == null || A.length < 3) {
        return 0;
    }

    int sum = 0;
    int n = A.length;
    int[] dp = new int[n];
    for (int i = 2; i < n; i++) {
        if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
            dp[i] = dp[i - 1] + 1;
            sum = sum + dp[i];
        } else {
            dp[i] = 0;
        }
    }

    return sum;
}
```
