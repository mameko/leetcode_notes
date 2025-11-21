# 560 Subarray Sum Equals K

Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [1,1,1], k = 2
</strong><strong>Output: 2
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [1,2,3], k = 3
</strong><strong>Output: 2
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 2 * 104`
* `-1000 <= nums[i] <= 1000`
* `-10^7 <= k <= 10^7`

好像这类比较难直接slide window的subarray都可以用[L138 Subarray Sum](l138-subarray-sum.md)的方法解决。138找的是equal 0，这里找到是equal k。这里需要注意的一点是，我们要用map而不是set。因为我们要记录等于某一个prefixsum的数目，然后加上。因为nums里有正有负，可能有重复的prefixsum。不加数目到map里的话，会漏掉答案的。T:O(n), S:O(n)

```java
public int subarraySum(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int prefixSum = 0;
    // <preSum, cnt>
    Map<Integer, Integer> sums = new HashMap<>();
    sums.put(0, 1);
    int count = 0;
    for (int i = 0; i < nums.length; i++) {
        prefixSum = prefixSum + nums[i];
        if (sums.containsKey(prefixSum - k)) {
            count = count + sums.get(prefixSum - k);
        }
        sums.put(prefixSum, sums.getOrDefault(prefixSum, 0) + 1);
    }

    return count;
}
```



