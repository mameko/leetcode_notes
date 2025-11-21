# 213 House Robber II

After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are **arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

## Notice

This is an extension of [House Robber](http://www.lintcode.com/problem/house-robber/).

**Example**

nums =`[3,6,4]`, return`6`

这题有环，采取第二种破环方式，2.断开算两边。基本跟上一题是一样的，这里我们只要分别算一边抢第一家，和抢最后一家的值，两者之间去最大的就行了。这里我们另外写了个函数来减少重复代码。注意下标的控制。

```java
public int rob(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    } else if (nums.length == 1) {
        return nums[0];
    }

    int n = nums.length;
    int max1 = helper(nums, 0, n - 2);
    int max2 = helper(nums, 1, n - 1);

    return Math.max(max1, max2);
}

private int helper(int[] nums, int s, int e) {
    if (s == e) {
        return nums[s];
    }

    int n = nums.length;
    int[] dp = new int[n];

    dp[s] = nums[s];
    dp[s + 1] = Math.max(nums[s], nums[s + 1]);
    for (int i = s + 2; i <= e; i++) {
        dp[i] = Math.max(dp[i - 2] + nums[i], dp[i - 1]);
    }

    return dp[e];
}
```

```java
public int houseRobber2(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    } else if (nums.length == 1) {
        return nums[0];
    }

    int n = nums.length;
    int rob0ToSencondLast = robHouseRange(nums, 0, n - 2);
    int rob1ToLast = robHouseRange(nums, 1, n - 1);
    return Math.max(rob0ToSencondLast, rob1ToLast);
}

private int robHouseRange(int[] nums, int s, int e) {
    int[] dp = new int[e + 1]; // make it as long as the original array - 1
    // need to assign dp according to start index,
    // or it will be hard to control since one starts from 1 the other starts from 0
    dp[s] = nums[s];
    dp[s + 1] = Math.max(nums[s], nums[s + 1]);
    for (int i = s + 2; i <= e; i++) {
        dp[i] = Math.max(nums[i] + dp[i - 2], dp[i - 1]);
    }

    return dp[e];
}
```

其实，老老实实写两个循环系最简单的：

```java
public int houseRobber2(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    } else if (nums.length == 1) {
        return nums[0];
    }

    int n = nums.length;
    int[] dp = new int[n];
    
    // 不抢第1家
    dp[0] = 0;
    dp[1] = nums[1];
    for(int i = 2; i < n; i++) {
        dp[i] = Math.max(dp[i -2] + nums[i], dp[i - 1]);
    }
    
    int answer = dp[n - 1];
    
    // 抢第1家
    dp[0] = 0;
    dp[1] = Math.max(nums[1], dp[0]);
    for(int i = 2; i < n; i++) {
        dp[i] = Math.max(dp[i -2] + nums[i], dp[i - 1]);
    } //最后n-2是因为我们抢了第一家，所以不能抢n-1
    
    
    return Math.max(dp[n - 2], answer);
}
```
