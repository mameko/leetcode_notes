# L43 Maximum Subarray III

Given an array of integers and a numbe&#x72;_&#x6B;_, find k`non-overlapping`subarrays which have the largest sum.

The number in each subarray should be**contiguous**.

Return the largest sum.

## Notice

The subarray should contain at least one number

**Example**

Given`[-1,4,-2,3,-2,3]`,`k=2`, return`8`

自己看注释

```java
public int maxSubArray(int[] nums, int k) {
    if (nums == null || k < 0 || nums.length < k) {
        return Integer.MIN_VALUE;
    }

    int n = nums.length;
    int[][] local = new int[k + 1][n + 1];// 到j这里的连续段的max
    int[][] global = new int[k + 1][n + 1];// 到j这里的不一定连续段的max

    for (int i = 1; i <= k; i++) {
        local[i][i - 1] = Integer.MIN_VALUE;//因为下面的代码要用，所以将前一个（k，k-1）初始化为MIN
        for (int j = i; j <= n; j++) {// 这段的大小从i到j，所以j从i开始到len
            // 能并进local的到这个数的那一段，所以k（i），or，作为一段加入global里，因为多了一段，所以global k - 1 (i - 1)
            // max（前一个localk段区间不加这个数时的max， 前一个global段区间里少一段区间时的max）+ 本区间值
            local[i][j] = Math.max(local[i][j - 1], global[i - 1][j - 1]) + nums[j - 1];
            if (i == j) { // 这是划分的区间等于一格，即使是负数也得加进去。
                global[i][j] = local[i][j];
            } else {
                // global 更新为，少一段时的max和多了这一段时的max中较大一个。
                global[i][j] = Math.max(global[i][j - 1], local[i][j]);
            }
        }
    }

    return global[k][n];
}
```
