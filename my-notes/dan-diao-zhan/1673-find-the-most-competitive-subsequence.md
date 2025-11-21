# 1673 Find the Most Competitive Subsequence

Given an integer array `nums` and a positive integer `k`, return _the most **competitive** subsequence of_ `nums` _of size_ `k`.

An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.

We define that a subsequence `a` is more **competitive** than a subsequence `b` (of the same length) if in the first position where `a` and `b` differ, subsequence `a` has a number **less** than the corresponding number in `b`. For example, `[1,3,4]` is more competitive than `[1,3,5]` because the first position they differ is at the final number, and `4` is less than `5`.

**Example 1:**

```
Input: nums = [3,5,2,6], k = 2
Output: [2,6]
Explanation: Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.
```

**Example 2:**

```
Input: nums = [2,4,3,3,5,4,9,6], k = 4
Output: [2,3,3,4]
```

**Constraints:**

* `1 <= nums.length <= 105`
* `0 <= nums[i] <= 109`
* `1 <= k <= nums.length`

这题，其实挺容易发现规律的，就是要找最小的，而且跟后面距离还有k的数，下一次找k - 1的，再下一次找k - 2的，如此类推。但用heap调了半天每弄对。然后看了答案，发现居然是单调栈。虽然用了deque来实现，但基本操作一样。

```java
public int[] mostCompetitive(int[] nums, int k) {
    if (nums == null || nums.length == 0 || nums.length <= k) {
        return nums;
    }

    Deque<Integer> dq = new ArrayDeque<>();
    // 要从nums里去掉n - k个数，组成这个sequence
    int removeCnt = nums.length - k;
    for (int i = 0; i < nums.length; i++) {
        while (!dq.isEmpty() && dq.peekLast() > nums[i] && removeCnt > 0) {
            dq.pollLast();
            removeCnt--;
        }
        dq.addLast(nums[i]);
    }

    int[] result = new int[k];
    for (int i = 0; i < k; i++) {
        result[i] = dq.pollFirst();
    }

    return result;
}
```
