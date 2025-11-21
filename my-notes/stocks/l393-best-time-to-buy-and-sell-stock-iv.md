# L393 Best Time to Buy and Sell Stock IV

Say you have an array for which th&#x65;_&#x69;\_th element is the price of a given stock on day\_i_.

Design an algorithm to find the maximum profit. You may complete at most`k`transactions.

## Notice

You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

**Example**

Given prices =`[4,4,6,1,1,4,2,5]`, and k =`2`, return`6`.

[**Challenge**](http://www.lintcode.com/en/problem/best-time-to-buy-and-sell-stock-iv/#challenge)

O(nk) time.

看注释

```java
public int maxProfit(int k, int[] prices) {
    if (k < 1 || prices == null || prices.length == 0) {
        return 0;
    }

    // if k >= prices.length, we can use buy sell stock 2's method
    if (prices.length <= k) {
        return maxProfit2(prices);
    }

    int res = Integer.MIN_VALUE;
    int n = prices.length;
    int[][] dp = new int[k + 1][n];
    // for (int i = 1; i <= k; i++) {
    //     for (int j = 1; j < n; j++) {
    //         dp[i][j] = dp[i][j - 1];// don't do tx at day j
    //         for (int m = 0; m < j; m++) {
    //             dp[i][j] = Math.max(dp[i][j], prices[j] - prices[m] + dp[i - 1][m]);//do a tx at day m
    //         }
    //     }
    // }


     for (int i = 1; i <= k; i++) {
        int diff = -prices[0];
        for (int j = 1; j < prices.length; j++) {
            dp[i][j] = dp[i][j - 1];//not selling at day j;
            dp[i][j] = Math.max(dp[i][j], prices[j] + diff);
            diff = Math.max(dp[i - 1][j] - prices[j], diff);
        }
    }
    /*
        for (int m = 0; m < j; m++) {
             dp[i][j] = Math.max(dp[i][j], prices[j] - prices[m] + dp[i - 1][m]);
        }

        we can use a diff to remember all : dp[i - 1][m] - prices[m] 
        everytime we add the maxdiff
        after adding we calculate the new maxdiff if necessary
    */
    return dp[k][n - 1];
}

private int maxProfit2(int[] prices) {
    int max = 0;
    for (int i = 1; i < prices.length; i++) {
        int diff = prices[i] - prices[i - 1];
        if (diff > 0) {
            max = max + diff;
        }
    }

    return max;
}
```
