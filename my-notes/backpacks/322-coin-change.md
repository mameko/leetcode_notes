# 322 Coin Change

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return`-1`.

**Example 1:**\
coins =`[1, 2, 5]`, amount =`11`\
return`3`(11 = 5 + 5 + 1)

**Example 2:**\
coins =`[2]`, amount =`3`\
return`-1`.

**Note**:\
You may assume that you have an infinite number of each kind of coin.

这题感觉似跟上一题很像，但是呢，要判断是否可行。然后再discuss里发现了一种跟backpackVI很类似的写法。有空得研究研究。补一个怎么从brute force到memo的推到过程。

这个问题，其实可以递归着做，譬如，我们要找min(11)，那么我们就可以先loop一遍coins，看看min(10), min(9), min(6)，哪个能返回最小值，然后，因为要多一步凑成11，所以min + 1。同理，把min(10)继续往下求，我们会求min(9),min(8),min(5)，多一步凑成10，所以min+1。我们会发现，有重复的步骤，min(9)，所以可以用memo来记录已经走过的分支。这里有两个base case，如果不可能凑成我们要返回-1，如果有0种可能凑成我们返回0。有不同的。T:O(n\*s)，每个min都要loop所有的coins， S:O(n)，递归的深度

2023.01，补一个datadog面经变体。求一个具体的min coin change方案。估计还是可以用memo来优化。被面试官各种提示才勉强写出了暴力的，没优化。具体做法是，recurHelper要返回最优方案，还要带一个tmp数组，来backtrack。在remain < 0的时候，返回 null。等于0的时候，返回传进来的tmp，tmp里记录了一个方案。后面，loop coin的时候，进入递归前，要把现在coin加到tmp里（现在想想，其实可以用一个数组，每次coin\[i]++，backtrack就coin\[i]--）出来要remove last，backtrack。如果返回的是null，那么跳过，证明我们减到负数，方案不成立。打擂台的时候，用返回值的长度，具体方案.size()来比较。还需要另外一个变量，当更新min的时候，更新。感觉memo也是用这个长度来update。最后返回的是最优的具体方案。

![](<../../.gitbook/assets/image (8).png>)

```cpp
public int coinChange(int[] coins, int amount) {
    if (coins == null || coins.length == 0 || amount < 1) {
        return 0;
    }
    
    Integer[] memo = new Integer[amount + 1];
    
    return recurHelper(memo, coins, amount);
}

private int recurHelper(Integer[] memo, int[] coins, int remain) {
    if (remain < 0) {
        return -1;
    }
    
    if (remain == 0) {
        return 0;
    }
    
    if (memo[remain] != null) {
        return memo[remain];
    }
    
    int min = Integer.MAX_VALUE;
    for (int coin : coins) {
        int cnt = recurHelper(memo, coins, remain - coin);
        if (cnt == -1) {
            continue;
        }
        min = Math.min(min, cnt + 1);
    }
    
    memo[remain] = min == Integer.MAX_VALUE ? -1 : min;
    return memo[remain];
}
```

```java
public int coinChange(int[] coins, int amount) {
    if (amount < 0 || coins == null || coins.length == 0)  {
        return -1;
    }

    int[][] dp = new int[coins.length + 1][amount + 1];

    for (int i = 0; i <= amount; i++) {
        dp[0][i] = Integer.MAX_VALUE;
    }

    for (int i = 1; i <= coins.length; i++) {
        for (int j = 1; j <= amount; j++) {
            dp[i][j] = dp[i - 1][j];
            // 第二个条件是，如果现在的值减去这个硬币之后，不能凑成整数，我们跳过。
            if (j >= coins[i - 1] && dp[i][j - coins[i - 1]] != Integer.MAX_VALUE) {
                dp[i][j] = Math.min(dp[i][j], dp[i][j - coins[i - 1]] + 1);
            }
        }
    }

    int res = dp[coins.length][amount];
    return (res == Integer.MAX_VALUE) ? -1 : res;
}
```

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int Max = amount + 1;
        vector<int> dp(amount + 1, Max);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.size(); j++) {
                if (coins[j] <= i) {
                    dp[i] = min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```
