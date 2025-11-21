# 795 Number of Subarrays with Bounded Maximum

Given an integer array `nums` and two integers `left` and `right`, return _the number of contiguous non-empty **subarrays** such that the value of the maximum array element in that subarray is in the range_ `[left, right]`.

The test cases are generated so that the answer will fit in a **32-bit** integer.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [2,1,4,3], left = 2, right = 3
</strong><strong>Output: 3
</strong><strong>Explanation: There are three subarrays that meet the requirements: [2], [2, 1], [3].
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [2,9,2,5,6], left = 2, right = 8
</strong><strong>Output: 7
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 10^5`
* `0 <= nums[i] <= 10^9`
* `0 <= left <= right <= 10^9`

这题一开始审错题了，还以为是subarray的和在range里。后来看了半天，原来是subarray里max的元素在range里。然后，就想到用cnt>=right的减去cnt>=（left-1）的。T:O(n),S:O(1)。这里因为我用了（n + 1）\* n / 2的方法，所以要long，不然会越界。看了答案，发现一直没掌握好这种直接加的方法。抄了一遍。有一丢丢像[L397 Longest Increasing Continuous Subsequence](../22-dp/l397-longest-increasing-continuous-subsequence.md),跟two sum里那种一次加一堆的也有一丢丢像。另外一个是[1887 Reduction Operations to Make the Array Elements Equal](../35-arrays/1887-reduction-operations-to-make-the-array-elements-equal.md)

<pre class="language-java"><code class="lang-java">// 更简单的写法
public int numSubarrayBoundedMax(int[] nums, int left, int right) {
    if (nums == null || nums.length == 0 || left > right) {
        return -1;
    }

    int lessThanLeft = cntMaxElemLessOrEqualToK(nums, left - 1);
    int lessThanRight = cntMaxElemLessOrEqualToK(nums, right);

    return lessThanRight - lessThanLeft;
}

private int cntMaxElemLessOrEqualToK(int[] nums, int k) {             
    int curLen = 0;
    int cnt = 0;
    for (int i = 0; i &#x3C; nums.length; i++) {            
        curLen = nums[i] &#x3C;= k ? curLen + 1 : 0;
        cnt = cnt + curLen;
    }

    return cnt;
}

// (n + 1) * n / 2，越界的方法
public int numSubarrayBoundedMax(int[] nums, int left, int right) {
<strong>    if (nums == null || nums.length == 0 || left > right) {
</strong>        return -1;
    }

    long lessThanLeft = cntMaxElemLessOrEqualToK(nums, left - 1);
    long lessThanRight = cntMaxElemLessOrEqualToK(nums, right);

    return (int) (lessThanRight - lessThanLeft);
}

private long cntMaxElemLessOrEqualToK(int[] nums, int k) {
    int left = 0;        
    long cnt = 0;
    for (int right = 0; right &#x3C;= nums.length; right++) {            
        if (right == nums.length || nums[right] > k) {
            long n = right - left;
            cnt = cnt + (n + 1) * n / 2;

            left = right + 1;                
        }
    }

    return cnt;
}
</code></pre>
