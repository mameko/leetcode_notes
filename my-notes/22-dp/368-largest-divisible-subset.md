# 368 Largest Divisible Subset

Given a set of`distinct positive`integers, find the largest subset such that every pair`(Si, Sj)`of elements in this subset satisfies:`Si % Sj = 0`or`Sj % Si = 0`.

## Notice

If there are multiple solutions, return any subset is fine.

**Example**

Given nums =`[1,2,3]`, return`[1,2]`or`[1,3]`

Given nums =`[1,2,4,8]`, return`[1,2,4,8]`

这题把数组里的数排序了以后，又转化为[L76](397-longest-increasing-continuous-subsequence.md)了。不过这里判断的条件不是数j大于数i，而是能否被整除。还是去\[j] + 1和本身两个中的max。注意还要用一个parent数组来把组成LIS的数给记住，因为最后返回结果是把这些数都找出来。

```java
public List<Integer> largestDivisibleSubset(int[] nums) {
    List<Integer> res = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return res;
    }

    int n = nums.length;
    // sort the array for easy division
    Arrays.sort(nums);

    int[] dp = new int[n];
    int[] parent = new int[n];// use to track where this comes from

    // initialize dp array, every one can be divided by itself so fill with 1
    Arrays.fill(dp, 1);

    // fill dp matrix when you can divide, just LIS
    for (int i = 0; i < n; i++) {
        // can init the dp[i] = 1 here instead to use Arrays.fill
        for (int j = 0; j < i; j++) {
            if (nums[i] % nums[j] == 0 && (dp[j] + 1) > dp[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
                parent[i] = j;
            }
        }
    }

    // find longest
    int maxcnt = 0;
    int maxInd = 0;
    for (int i = 0; i < n; i++) {
        if (dp[i] > maxcnt) {
            maxcnt = dp[i];
            maxInd = i;
        }
    }

    // collect nums into res
    for (int i = 0; i < maxcnt; i++) {
        res.add(nums[maxInd]);
        maxInd = parent[maxInd];
    }

    return res;

}
```
