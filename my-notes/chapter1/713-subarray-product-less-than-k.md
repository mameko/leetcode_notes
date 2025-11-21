# 713 Subarray Product Less Than K

Your are given an array of positive integers `nums`.

Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than `k`.

**Example 1:**

```
Input: nums = [10, 5, 2, 6], k = 100
Output: 8
Explanation: The 8 subarrays that have product less than 100 are: [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
```

**Note:**

`0 < nums.length <= 50000`.

`0 < nums[i] < 1000`.

`0 <= k < 10^6`.

这题，很难，想了半天。看了hint，虽然知道是sliding window和可能要用prefixProduct，但一直做不出来。其实这里有两个解法，第一个是利用prefixProd来二分。先定住右边端点，然后二分找最小的满足条件的left。然后每次找到我们加right - left + 1到结果。这里用了log来把prod转化成sum。就写下来参考一下。T:O(nlogn)，S:O(n).主要还是第二个解法，sliding window。我们一边向右移动window，一边加上right - left + 1。每次多一个符合条件的数，要把那个数加上，然后加上那个数与前面那些数组成符合条件的数。T:O(n), S: O(1)

```java
// 解法一：
public int numSubarrayProductLessThanK(int[] nums, int k) {
    if (k == 0) return 0;
    double logk = Math.log(k);
    double[] prefix = new double[nums.length + 1];
    for (int i = 0; i < nums.length; i++) {
        prefix[i+1] = prefix[i] + Math.log(nums[i]);
    }

    int ans = 0;
    for (int i = 0; i < prefix.length; i++) {
        int lo = i + 1, hi = prefix.length;
        while (lo < hi) {
            int mi = lo + (hi - lo) / 2;
            if (prefix[mi] < prefix[i] + logk - 1e-9) lo = mi + 1;
            else hi = mi;
        }
        ans += lo - i - 1;
    }
    return ans;
}

// 解法二：
public int numSubarrayProductLessThanK(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k <= 1) {
        return 0;
    }
    
    int prod = 1;
    int left = 0;
    int total = 0;
    for (int right = 0; right < nums.length; right++) {
        prod = prod * nums[right];
        while (prod >= k) {
            prod = prod / nums[left];
            left++;
        }
        total = total + right - left + 1;
    }
    
    return total;
}
```
