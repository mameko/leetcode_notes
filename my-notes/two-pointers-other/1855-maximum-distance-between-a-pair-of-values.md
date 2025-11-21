# 1855 Maximum Distance Between a Pair of Values

You are given two **non-increasing 0-indexed** integer arrays `nums1`​​​​​​ and `nums2`​​​​​​.

A pair of indices `(i, j)`, where `0 <= i < nums1.length` and `0 <= j < nums2.length`, is **valid** if both `i <= j` and `nums1[i] <= nums2[j]`. The **distance** of the pair is `j - i`​​​​.

Return _the **maximum distance** of any **valid** pair_ `(i, j)`_. If there are no valid pairs, return_ `0`.

An array `arr` is **non-increasing** if `arr[i-1] >= arr[i]` for every `1 <= i < arr.length`.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums1 = [55,30,5,4,2], nums2 = [100,20,10,10,5]
</strong><strong>Output: 2
</strong><strong>Explanation: The valid pairs are (0,0), (2,2), (2,3), (2,4), (3,3), (3,4), and (4,4).
</strong>The maximum distance is 2 with pair (2,4).
</code></pre>

**Example 2:**

<pre><code><strong>Input: nums1 = [2,2,2], nums2 = [10,10,1]
</strong><strong>Output: 1
</strong><strong>Explanation: The valid pairs are (0,0), (0,1), and (1,1).
</strong>The maximum distance is 1 with pair (0,1).
</code></pre>

**Example 3:**

<pre><code><strong>Input: nums1 = [30,29,19,5], nums2 = [25,25,25,25,25]
</strong><strong>Output: 2
</strong><strong>Explanation: The valid pairs are (2,2), (2,3), (2,4), (3,3), and (3,4).
</strong>The maximum distance is 2 with pair (2,4).
</code></pre>

&#x20;

**Constraints:**

* `1 <= nums1.length, nums2.length <= 105`
* `1 <= nums1[i], nums2[j] <= 105`
* Both `nums1` and `nums2` are **non-increasing**.

这个题，很经典的双指针，而且有点2sum的感觉，只是这里的判断条件不是sum == target，而是i <= j && nums1\[i] <= nums2\[j]。如果符合条件，我们算max，如果不符合的时候，我们移动指针。因为条件限制了i <= j，然后max是算j - i，所以j一定要比i大，不符合条件，同时向右。然后还有一种情况是，i <= j, j到了结尾了。这种情况也不需要特判，因为j - i求的是max，就算继续移动i向右也不可能得到更大的值。所以两个条件都归纳到else里了。T:O(n + m)，S: O(1)

```java
public int maxDistance(int[] nums1, int[] nums2) {
    if (nums1 == null || nums2 == null || 
        nums1.length == 0 || nums2.length == 0) {
        return -1;
    }

    int max = 0;
    int i = 0;
    int j = 0;
    while (i < nums1.length && j < nums2.length) {
        if (i <= j && nums1[i] <= nums2[j]) {
            max = Math.max(j - i, max);
            j++;
        } else {
            i++;
            j++;
        }
    }        

    return max;
}
```
