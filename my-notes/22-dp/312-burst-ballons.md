# 312 Burst Ballons

Given`n`balloons, indexed from`0`to`n-1`. Each balloon is painted with a number on it represented by array`nums`. You are asked to burst all the balloons. If the you burst balloon`i`you will get`nums[left] * nums[i] * nums[right]`coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the`maximum`coins you can collect by bursting the balloons wisely.

* You may imagine`nums[-1] = nums[n] = 1`. They are not real therefore you can not burst them. &#x20;
* 0 ≤`n`≤ 500, 0 ≤`nums[i]`≤ 100

**Example**

Given`[4, 1, 5, 10]`\
Return`270`

```
nums = [4, 1, 5, 10] burst 1, get coins 4 * 1 * 5 = 20
nums = [4, 5, 10]    burst 5, get coins 4 * 5 * 10 = 200 
nums = [4, 10]       burst 4, get coins 1 * 4 * 10 = 40
nums = [10]          burst 10, get coins 1 * 10 * 1 = 10

Total coins 20 + 200 + 40 + 10 = 270
```

这题是划分类，所以比较适合从大到小用dfs记忆化搜索。首先我们要找区间\[0, n - 1]\(整个区间的)。进到递归函数，如果我们已经算过这个区间的话直接返回，如果没有才继续算下去。然后我们来到base case，区间长度为1，只有一个气球。这个是第一个被打爆的气球。分情况来算分，因为一开始的那个和最后那个要特别处理。然后我们得处理其他区间长度大于1的情况。基本上，我们枚举区间的断点，算算断点左边的分数，再算算断点右边的分数。然后还得算算断点处的分数。这里也得注意第一个和最后一个的特殊情况。然后把3部分的分数加起来。然后，把所有断点loop一遍，找出可以得到的最高分数。把那个分数记录到dp矩阵里。（s 到 e这段区间里能打到的最大值）

再加一个follow up，如果是个环，怎么破。这个的做法跟[L593 Stone Game II](l593-stone-game-ii.md)一样，变成2n，就是最前面和最后面不是乘以1了，而是乘以相应的球的值

```java
public int maxCoins(int[] nums) {
     if (nums == null || nums.length == 0) {
         return 0;
     }

     int n = nums.length;
     if (n == 1) {
         return nums[0];
     }

     int[][] dp = new int[n][n];
     dfs(0, n - 1, dp, nums);
     return dp[0][n - 1];
}

private int dfs(int s, int e, int[][] dp, int[] nums) {
    // if already calculated, return directly
    if (dp[s][e] > 0) {
        return dp[s][e];
    }

    // 1st one to burst, base case
    if (s == e) {
        if (s == 0) {// 1st ballon
            dp[s][e] = 1 * nums[s] * nums[s + 1];
        } else if (s == nums.length - 1) {// last ballon
            dp[s][e] = nums[s - 1] * nums[s] * 1;
        } else {// baloons in the middle
            dp[s][e] = nums[s - 1] * nums[s] * nums[s + 1];
        }
    } else { // other cases
        // loop all the possible last balloon to burst position in this
        // range
        int max = 0;
        int startLeft = (s == 0) ? 1 : nums[s - 1];// 左边气球的值
        int endRight = (e == nums.length - 1) ? 1 : nums[e + 1];// 右边气球的值
        for (int i = s; i <= e; i++) {
            // find the max and assign to dp[s][e]
            int leftPoints = (s == i) ? 0 : dfs(s, i - 1, dp, nums);//左区间得到的最大值
            int rightPoints = (i == e) ? 0 : dfs(i + 1, e, dp, nums);//右区间得到的最大值

            int midPoints = startLeft * nums[i] * endRight;// 打爆i气球得到的值
            int cur = leftPoints + rightPoints + midPoints;// 加起来跟max比较大小
            max = Math.max(max, cur);
        }
        dp[s][e] = max;
    }

    return dp[s][e];
}
```

补一个递推的

```java
public class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n + 2][n + 2];
        
        // 做一个偏移，前面加一个1后面加一个1
        nums = Arrays.copyOf(nums, nums.length + 2);
        for (int i = n - 1; i >= 0; i--) {
            nums[i + 1] = nums[i];
        }        
        nums[0] = 1;
        nums[n + 1] = 1;
        
        for (int len = 1; len <= n; len++) {
            for (int i = 1; i <= n && i + len -1 <= n; i++) {
                int j = i + len - 1;
                for (int k = i; k <= j; k++) {
                    dp[i][j] = Math.max(dp[i][k - 1] + dp[k + 1][j] + nums[i -1] * nums[k] * nums[j + 1], dp[i][j]);
                }
            }
        }
        return dp[1][n];
    }
}    
        
```
