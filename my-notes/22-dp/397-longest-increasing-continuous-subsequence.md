# L76 Longest Increasing Subsequence

Give an integer array，find the longest increasing continuous subsequence in this array.

An increasing continuous subsequence:

* Can be from right to left or from left to right.
* Indices of the integers in the subsequence should be continuous.

## Notice

O(n) time and O(1) extra space.

**Example**

For`[5, 4, 2, 1, 3]`, the LICS is`[5, 4, 2, 1]`, return`4`.

For`[5, 1, 2, 3, 4]`, the LICS is`[1, 2, 3, 4]`, return`4`.

用一个以为的dp来记录到现在这个地方为止的最长递增subsequence。dp初始化为1，因为最短的LIS就是一个数字。每次都要go through前面一堆数字until现在的i。然后每当找到比现在数字小的j时，就更新dp。取dp与 \[j] + 1中较大值。最后结果从dp数列中找max，因为max不一定是最后一个数字。T:O(n^2), S:O(n)。另外还能用nlogn的二分来做。

```java
public int longestIncreasingSubsequence(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int[] dp = new int[nums.length];

    int max = Integer.MIN_VALUE;
    for (int i = 0; i < nums.length; i++) {
        dp[i] = 1; // init to 1
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[j] + 1, dp[i]);
            }
        }
        max = Math.max(dp[i], max);
    }

    return max;
}
```
