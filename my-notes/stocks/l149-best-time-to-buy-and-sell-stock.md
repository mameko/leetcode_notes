# L149 Best Time to Buy and Sell Stock

Say you have an array for which th&#x65;_&#x69;\_thelement is the price of a given stock on day\_i_.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

**Example**

Given array`[3,2,3,1,2]`, return`1`.

因为只能做一次交易，所以一边loop一边找min（买入点）。用现在的价钱-min 算一个差值（表示今天卖出），max这个差值就是结果。T:O(n)，S:O(1)

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length == 0) {
        return 0;
    }

    int buy = Integer.MAX_VALUE;
    int res = Integer.MIN_VALUE;
    for (int p : prices) {
        buy = Math.min(p, buy);
        res = Math.max(res, p - buy);
    }

    return res;
}
```
