# 1099 Two Sum Less Than K

Given an array `nums` of integers and integer `k`, return the maximum `sum` such that there exists `i < j` with `nums[i] + nums[j] = sum` and `sum < k`. If no `i`, `j` exist satisfying this equation, return `-1`.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [34,23,1,24,75,33,54,8], k = 60
</strong><strong>Output: 58
</strong><strong>Explanation: We can use 34 and 24 to sum 58 which is less than 60.
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [10,20,30], k = 15
</strong><strong>Output: -1
</strong><strong>Explanation: In this case it is not possible to get a pair sum less that 15.
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 100`
* `1 <= nums[i] <= 1000`
* `1 <= k <= 2000`

这题一开始没认真审题，以为是[L609 Two Sum V](l609-two-sum-v.md)认真看了才发现，原来找的是closet，所以去看了[16 3 Sum Closest](16-3-sum-closest.md)。但其实更像[L553 Two Sum Closest](l553-two-sum-closest.md)这里自己维护了一个diff，看了答案，其实可以直接用一个max来track。这个max只能在less than的时候才ok。L553那里可能是above的。T:O(nlogn), S:O(1)

<pre class="language-java"><code class="lang-java"><strong>// my 解法
</strong><strong>public int twoSumLessThanK(int[] nums, int k) {
</strong>    if (nums == null || nums.length == 0) {
        return -1;
    }

    int left = 0;
    int right = nums.length - 1;
    Arrays.sort(nums);
    int ans = -1;
    int minDiff = Integer.MAX_VALUE;
    while (left &#x3C; right) {
        int sum = nums[left] + nums[right];
        int diff = Math.abs(k - sum);
        if (sum &#x3C; k) {
            if (diff &#x3C; minDiff) {
                minDiff = diff;
                ans = sum;
            }
            left++;
        } else if (sum >= k) {
            right--;
        }
    }

    return ans;
}

// leetcode 答案
public int twoSumLessThanK(int[] nums, int k) {
    Arrays.sort(nums);
    int answer = -1;
    int left = 0;
    int right = nums.length - 1;
    while (left &#x3C; right) {
        int sum = nums[left] + nums[right];
        if (sum &#x3C; k) {
            answer = Math.max(answer, sum);
            left++;
        } else {
            right--;
        }
    }
    return answer;
}
</code></pre>

