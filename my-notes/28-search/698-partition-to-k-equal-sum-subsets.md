# 698 Partition to K Equal Sum Subsets

Given an array of integers`nums`and a positive integer`k`, find whether it's possible to divide this array into`k`non-empty subsets whose sums are all equal.

**Example 1:**

```
Input: nums = [4, 3, 2, 3, 5, 2, 1], k = 4
Output: True

Explanation: It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.
```

**Note:**

* `1 <= k <= len(nums) <= 16`.
* `0 < nums[i] < 10000`.

这题好像有比较fancy的solution，感觉是记不住的。这里我只用了深搜，很暴力地搜。这题感觉像是[416 Partition Equal Subset Sum](../backpacks/416-partition-equal-subset-sum.md)的follow up。前一题能用dp，这题...感觉还是深搜比较好理解。

至于复杂度嘛...have no idea。感觉有点像[37 Sudoku solver](37-sudoku-solver.md)。粗略估算，然后因为要找k个subset，所以起码k层，然后每一层的深度depends on target，如果target大的话，最坏情况可能有O(N)，就是全部数字都加上才找到target。然后时间，就真难算，因为用了continue去排除一些一定不合法的选项。每次combination sum会用2^N，然后要求k个，所以O(K\*2^n)

```java
public boolean canPartitionKSubsets(int[] nums, int k) {
    if (nums == null || nums.length == 0 || k < 0) {
        return false;
    }

    int total = 0;
    for (int num : nums) {
        total += num;
    }

    if (total % k != 0) { // 如果不能平均分成k份，直接返回false
        return false;
    }

    boolean[] visited = new boolean[nums.length];// 因为元素不能重复取，所以用visited来区分
    return dfs(nums, k, visited, total/k, total/k, 0);
}

private boolean dfs(int[] nums, int k, boolean[] visited, int target, int original, int start) {
    if (k == 1) {// 如果k到了1，证明我们已经找到k - 1个，而且我们知道是能被分成k个的，最后一个不用找了。
        return true;
    }

    // 如果我们找到了一个subset，k - 1，继续在其他的数字里找subset，这个original传进来记住我们初始要找的target的值
    if (target == 0) { 
        return dfs(nums, k - 1, visited, original, original, 0);
    }

    for (int i = start; i < nums.length; i++) { // 如果没找到target，我们继续往下找
        if (nums[i] > target || visited[i]) { // 这里是跳过不合法的取法，例如会把target减成负数的，或者已经用过的数字
            continue;
        }
        visited[i] = true;  // 取了数字，记录一下用过          
        // 然后我们继续往下找，这时候还是得找k个subset，因为还在填现在的这个subset
        if (dfs(nums, k, visited, target - nums[i], original, i + 1)) {// i + 1是因为不能重复取
            return true; // 如果找到，证明我们hit base case，返回true
        }
        visited[i] = false; // 如果找不到，我们backtrack
    }

    return false; // 如果全部试了都不行就返回false
}
```
