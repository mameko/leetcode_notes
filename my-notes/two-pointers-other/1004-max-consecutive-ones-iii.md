# 1004 Max Consecutive Ones III

Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.

&#x20;**Example 1:**

<pre><code><strong>Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
</strong><strong>Output: 6
</strong><strong>Explanation: [1,1,1,0,0,1,1,1,1,1,1]
</strong>Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
</code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
</strong><strong>Output: 10
</strong><strong>Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
</strong>Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.
</code></pre>

&#x20;**Constraints:**

* `1 <= nums.length <= 105`
* `nums[i]` is either `0` or `1`.
* `0 <= k <= nums.length`

在做2的时候，已经想到3这个follow up了。

一开始还以为是要dp，因为有很多重复子问题，而且如果你有连续3个0，k=2，那么怎么知道填哪两个能给出最优解呢？但感觉复杂度还是有点高，因为数一次max要O(n)，然后dp好像还得3维？到底是前缀型的，还是区间型的？最后想不通，所以放弃了dp了。

然后寻思着，这个max最大不就是len吗？那么能不能二分答案呢？但同样，没想通，因为判断能否可行的函数要尝试所有0的位置，感觉复杂度也不低。

最后，看答案前瞅了一眼tag，有点灵感了。prefixsum和sliding window。然后，尝试了例子后想通了。因为我们可以通过下标跟prefixsum的差之间的关系，推敲出中间缺了多少个0。如果，我们的差小于等于k那么说明，我们这一段right - left之间是可以全要的，算max。如果>k，证明我们0不够用了，就要挪动做指针，找到下一段位置。我猜想，因为这要求连续的，所以可以用sliding window解决而不是dp。T:O(n), S:O(n)

看了答案之后，还有一个领悟。之前一直对这类题，左移指针时，为什么可以if或者while都行有个疑惑。其实是因为，我们要求max，所以以后每个小于之前一轮的max的解我们都不会去取。所以左右指针可以同时移动，所以if也是ok的。

过了几天之后，又想了想，其实我们不需要维护这个prefixSum数组。因为是连续的，所以我们可以用一个sum变量记录就行了， 就像[L604 Window Sum](../chapter1/l604-window-sum.md)那样。这样就可以省空间。不过要注意的是，下标又一丢丢的变化，得算好。

```java
// windows sum的
public int longestOnes(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0) {
        return 0;
    }

    int max = 0;
    int left = 0;
    int sum = 0;
    for (int right = 0; right < nums.length; right++) {
        sum = sum + nums[right];
        int diff = right - left + 1 - sum;
        if (diff <= k) {
            max = Math.max(max, right - left + 1);
            continue;
        } 
        // if or while都ok
        if (diff > k && left < right) {
            sum = sum - nums[left];
            left++;
            diff = right - left + 1 - sum;
        }
    }
    return max;
}


// prefix sum的
public int longestOnes(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0) {
        return 0;
    }

    // calculate prefixSum
    int[] prefixSum = new int[nums.length + 1];
    for (int i = 1; i < nums.length + 1; i++) {
        prefixSum[i] = prefixSum[i - 1] + nums[i - 1];
    }

    // sliding windows on prefixSum
    int max = 0;
    int left = 0;
    for (int right = 1; right < nums.length + 1; right++) {
        int diff = right - left - (prefixSum[right] - prefixSum[left]);
        if (diff <= k) {
            max = Math.max(max, right - left);
            continue;
        } 

        // if or while都ok
        if (diff > k && left < right) {
            left++;
            diff = right - left - (prefixSum[right] - prefixSum[left]);
        }
    }
    return max;
}
```
