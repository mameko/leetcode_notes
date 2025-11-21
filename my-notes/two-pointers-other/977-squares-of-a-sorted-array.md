# 977 Squares of a Sorted Array

Given an integer array `nums` sorted in **non-decreasing** order, return _an array of **the squares of each number** sorted in non-decreasing order_.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [-4,-1,0,3,10]
</strong><strong>Output: [0,1,9,16,100]
</strong><strong>Explanation: After squaring, the array becomes [16,1,0,9,100].
</strong>After sorting, it becomes [0,1,9,16,100].
</code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [-7,-3,2,3,11]
</strong><strong>Output: [4,9,9,49,121]
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= nums.length <= 104`
* `-104 <= nums[i] <= 104`
* `nums` is sorted in **non-decreasing** order.

&#x20;

**Follow up:** Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?

这题，一看最简单的解法是直接sqrt然后sort，nlogn。后来看了solution，发现一个比较有趣的做法，2ptr。因为本来是sorted的array。所以一个指前，一个指尾。如果abs大的，就先sqrt and place在result array的最后。这样就可以O(n)了。

```java
//n
public int[] sortedSquares(int[] nums) {
    if (nums == null || nums.length == 0) {
        return null;
    }

    int size = nums.length;
    int[] res = new int[size];
    int start = 0;
    int end = size - 1;
    for (int i = size - 1; i >= 0; i--) {            
        if (Math.abs(nums[start]) > Math.abs(nums[end])) {
            res[i] = nums[start] * nums[start];
            start++; 
        } else {
            res[i] = nums[end] * nums[end];
            end--; 
        }
    }

    return res;
}


// nlogn
public int[] sortedSquares(int[] nums) {
    if (nums == null || nums.length == 0) {
        return null;
    }

    int size = nums.length;
    int[] res = new int[size];
    for (int i = 0; i < size; i++) {
        int sqNum = nums[i] * nums[i];
        res[i] = sqNum;
    }

    Arrays.sort(res);
    return res;
}
```
