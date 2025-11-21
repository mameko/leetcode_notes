# 1005 Maximize Sum Of Array After K Negations

Given an integer array `nums` and an integer `k`, modify the array in the following way:

* choose an index `i` and replace `nums[i]` with `-nums[i]`.

You should apply this process exactly `k` times. You may choose the same index `i` multiple times.

Return _the largest possible sum of the array after modifying it in this way_.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [4,2,3], k = 1
</strong><strong>Output: 5
</strong><strong>Explanation: Choose index 1 and nums becomes [4,-2,3].
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [3,-1,0,2], k = 3
</strong><strong>Output: 6
</strong><strong>Explanation: Choose indices (1, 2, 2) and nums becomes [3,1,0,2].
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: nums = [2,-3,-1,5,-4], k = 2
</strong><strong>Output: 13
</strong><strong>Explanation: Choose indices (1, 4) and nums becomes [2,3,-1,5,4].
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 104`
* `-100 <= nums[i] <= 100`
* `1 <= k <= 104`

一开始看例子的时候，以为做法是排序，然后把负数都变正了，然后多出来的k会用来flip最小的那个正数。后来有一个test case过不了。\[-8, -5, -5, -3, -2, 3]， k=6，我的做法会把所有负数都变成正数，然后把最小的正数，所有结果会是，\[8, 5, 5, 3, 2, -3] = 20, 但正确解法是，把2flip两次，\[8, 5, 5, 3, -2, 3]=22。后来再思考了一下，发现，其实flip的是，全变成正数后，最小的那个正数。如果k是偶数，那么不用变，如果k是奇数，那么flip最小那个数能得到最大的sum。

一开始，sort了两次，第二次是为了找最小的正数。后来看了discussion，大佬直接在sum的时候，把最小的记录下来。返回时，根据k的奇偶减掉min就ok了，注意减的时候因为之前已经加了一次，所以减的是min\*2。T: O(n),排序，S:O(1）

其实，一开始的时候还在寻思着会不会有更优的解法，后来看了tag，发现真的是greedy，就放心sort了。

```java
public int largestSumAfterKNegations(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0) {
        return 0;
    }

    Arrays.sort(nums);

    int len = nums.length;
    int i = 0; 
    while (i < len && k > 0 && nums[i] < 0) {
        nums[i] = -nums[i];
        i++;
        k--;
    }

    int sum = 0;
    int min = Integer.MAX_VALUE;
    for (int j = 0; j < len; j++) {
        sum += nums[j];
        min = Math.min(min, nums[j]);
    }

    return k % 2 == 0 ? sum : sum - min * 2;
}
```
