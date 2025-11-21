# 1918 Kth Smallest Subarray Sum

Given an integer array `nums` of length `n` and an integer `k`, return _the_ `kth` _**smallest subarray sum**._

A **subarray** is defined as a **non-empty** contiguous sequence of elements in an array. A **subarray sum** is the sum of all elements in the subarray.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [2,1,3], k = 4
</strong><strong>Output: 3
</strong><strong>Explanation: The subarrays of [2,1,3] are:
</strong>- [2] with sum 2
- [1] with sum 1
- [3] with sum 3
- [2,1] with sum 3
- [1,3] with sum 4
- [2,1,3] with sum 6 
Ordering the sums from smallest to largest gives 1, 2, 3, 3, 4, 6. The 4th smallest is 3.
</code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [3,3,5,5], k = 7
</strong><strong>Output: 10
</strong><strong>Explanation: The subarrays of [3,3,5,5] are:
</strong>- [3] with sum 3
- [3] with sum 3
- [5] with sum 5
- [5] with sum 5
- [3,3] with sum 6
- [3,5] with sum 8
- [5,5] with sum 10
- [3,3,5], with sum 11
- [3,5,5] with sum 13
- [3,3,5,5] with sum 16
Ordering the sums from smallest to largest gives 3, 3, 5, 5, 6, 8, 10, 11, 13, 16. The 7th smallest is 10.
</code></pre>

&#x20;

**Constraints:**

* `n == nums.length`
* `1 <= n <= 2 * 104`
* `1 <= nums[i] <= 5 * 104`
* `1 <= k <= n * (n + 1) / 2`

这题，想了半天没想出来，因为brute force是n^2级别的，所以继续优化就只有二分，或者sliding windows了。因为一直想不出来怎么不brute force所有值，然后扔掉一半。后来看了提示，二分答案之前，可以用sliding window算小于等于k的subarray有多少个。但这个又卡了半天，后来想起了[713 Subarray Product Less Than K](713-subarray-product-less-than-k.md). 这里可以通过O(n)求出subarray的个数。这里注意得**正数**才能这么干，因为从left到right之间的所有subarray end with right都是符合条件的，所有可以一次加上。有负数就不行了。另外一个需要注意的地方是，二分找什么。这个研究了好久。譬如，k=7，如果我们找到小于等于X的subarray有6个，那么，我们知道X要往后（大的）找，所以start = mid。如果我们找到小于等于X的subarray有8个，那么我们知道要往前（小的）找。triky的地方是，等于的时候，不能返回，要继续往前找。而且最后特判start和end的时候，要用>=k，而不是=。总体上感觉这题是找**第一个符合条件**的二分答案。T:O(n \* log(range)), S:O(1)

```java
public int kthSmallestSubarraySum(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 1) {
        return 0;
    }

    int total = 0;
    int min = Integer.MAX_VALUE;
    for (int i = 0; i < nums.length; i++) {
        total = total + nums[i];
        min = Math.min(min, nums[i]);
    }

    int start = min;
    int end = total;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (countSumLessOrEqualToTarget(nums, mid) > k) {
            end = mid;
        } else if (countSumLessOrEqualToTarget(nums, mid) < k) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (countSumLessOrEqualToTarget(nums, start) >= k) {
        return start;
    }

    if (countSumLessOrEqualToTarget(nums, end) >= k) {
        return end;
    }

    return 0;
}

private int countSumLessOrEqualToTarget(int[] nums, int target) {
    int sum = 0;
    int count = 0;
    int left = 0;
    for (int right = 0; right < nums.length; right++) {
        sum = sum + nums[right];
        while (sum > target) {
            sum = sum - nums[left];
            left++;
        }
        count = count + right - left + 1;
    }
    return count;
}
```
