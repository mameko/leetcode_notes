# 2395 Find Subarrays With Equal Sum

Given a **0-indexed** integer array `nums`, determine whether there exist **two** subarrays of length `2` with **equal** sum. Note that the two subarrays must begin at **different** indices.

Return `true` _if these subarrays exist, and_ `false` _otherwise._

A subarray is a contiguous non-empty sequence of elements within an array.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [4,2,4]
</strong><strong>Output: true
</strong><strong>Explanation: The subarrays with elements [4,2] and [2,4] have the same sum of 6.
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [1,2,3,4,5]
</strong><strong>Output: false
</strong><strong>Explanation: No two subarrays of size 2 have the same sum.
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: nums = [0,0,0]
</strong><strong>Output: true
</strong><strong>Explanation: The subarrays [nums[0],nums[1]] and [nums[1],nums[2]] have the same sum of 0. 
</strong>Note that even though the subarrays have the same content, the two subarrays are considered different because they are in different positions in the original array.
</code></pre>

&#x20;

**Constraints:**

* `2 <= nums.length <= 1000`
* `-109 <= nums[i] <= 109`

这题主要是1477 Find Two Non-overlapping Sub-arrays Each With Target Sum原题做不通，然后点开相关题目过来看看有啥灵感才做的。这里因为规定了长度为2，而且subarray可以overlap。所以直接loop一圈，两两算一个和，用set查重。找到了就return。T:O(n), S:O(n)

```java
public boolean findSubarrays(int[] nums) {
    if (nums == null || nums.length == 0) {
        return false;
    }

    // <sum>
    Set<Integer> set = new HashSet<>();
    for (int i = 0; i < nums.length - 1; i++) {
        int sum = nums[i] + nums[i + 1];
        if (set.contains(sum)) {
            return true;
        }
        set.add(sum);
    }

    return false;
}
```
