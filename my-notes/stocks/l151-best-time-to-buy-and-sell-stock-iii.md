# L151 Best Time to Buy and Sell Stock III

Say you have an array for which th&#x65;_&#x69;\_thelement is the price of a given stock on day\_i_.

Design an algorithm to find the **maximum** profit. You may complete at most\_two\_transactions.

## Notice

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).

**Example**

Given an example`[4,4,6,1,1,4,2,5]`, return`6`.

这题有两个解法

解法一：prefix sum跟前面做过的[Maximum Subarrays](../max-subarrays/)做法相似。

```java
// method 1, do stock 1 from left to right, and right to left then add them together
public int maxProfit(int[] prices) {
    if (prices == null || prices.length == 0 || prices.length == 1) {
        return 0;
    }

    // we want to find a point j in the days, so that profit in these 2 periods ( [0..j] + [j..n - 1] ) is max
    int n = prices.length;
    int[] left = new int[n];// keep max profit before i
    int[] right = new int[n];// keep max profit after i

    // find max profit by buying at min
    int leftBuy = prices[0];
    for (int i = 1; i < n; i++) {
        leftBuy = Math.min(leftBuy, prices[i]);
        left[i] = Math.max(left[i - 1], prices[i] - leftBuy);
    }

    // find max profit buy selling at max
    int rightSell = prices[n - 1];
    for (int i = n - 2; i >= 0; i--) {
        rightSell = Math.max(rightSell, prices[i]);
        right[i] = Math.max(right[i + 1], rightSell - prices[i]);
    }

    int res = 0;
    for (int i = 0; i < n; i++) {
        res = Math.max(res, left[i] + right[i]);
    }

    return res;
}
```

解法二：跟stock 4 一样，DP

```java
// method 2, like stock IV
public int maxProfit(int[] prices) {
    if (prices == null || prices.length == 0 || prices.length == 1) {
        return 0;
    }

    if (prices.length <= 2) {
        return maxProfit2(prices);
    }

    int[][] dp = new int[3][prices.length];

    // for (int i = 1; i < 3; i++) {
    //     for (int j = 1; j < prices.length; j++) {
    //         dp[i][j] = dp[i][j - 1];//not selling at day j;
    //         for (int k = 0; k < j; k++) {
    //             dp[i][j] = Math.max(dp[i][j], prices[j] - prices[k] + dp[i - 1][k]);
    //         }
    //     }
    // }

    for (int i = 1; i < 3; i++) {
        int diff = -prices[0];
        for (int j = 1; j < prices.length; j++) {
            dp[i][j] = dp[i][j - 1];//not selling at day j;
            dp[i][j] = Math.max(dp[i][j], prices[j] + diff);
            diff = Math.max(dp[i - 1][j] - prices[j], diff);
        }
    }
    /*
        for (int k = 0; k < j; k++) {
             dp[i][j] = Math.max(dp[i][j], prices[j] - prices[k] + dp[i - 1][k]);
        }

        we can use a diff to remember all : dp[i - 1][k] - prices[k] 
        everytime we add the maxdiff
        after adding we calculate the new maxdiff if necessary
    */

    return dp[2][prices.length - 1];
}

public int maxProfit2(int[] prices) {
    if (prices == null || prices.length == 0) {
        return 0;
    }

    int res = 0;
    for (int i = 1; i < prices.length; i++) {
        int diff = prices[i] - prices[i - 1];
        if (diff > 0) {
            res += diff;
        }
    }

    return res;
}
```
