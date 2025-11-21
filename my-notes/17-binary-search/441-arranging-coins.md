# 441 Arranging Coins



You have `n` coins and you want to build a staircase with these coins. The staircase consists of `k` rows where the `ith` row has exactly `i` coins. The last row of the staircase **may be** incomplete.

Given the integer `n`, return _the number of **complete rows** of the staircase you will build_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/arrangecoins1-grid.jpg)

```
Input: n = 5
Output: 2
Explanation: Because the 3rd row is incomplete, we return 2.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/09/arrangecoins2-grid.jpg)

```
Input: n = 8
Output: 3
Explanation: Because the 4th row is incomplete, we return 3.
```

&#x20;

**Constraints:**

* `1 <= n <= 231 - 1`

这题写着简单，但是很容易overflow。首先，一眼看出这是个（1 + k) \* k / 2 <= n的题。一开始为了避开java里正数除以2会round down的问题，用了（1 + k) \* k <= n \* 2。然后后面大数overflow了。还是套九章的模板。

```java
public int arrangeCoins(int n) {
    if (n < 1) {
        return 0;
    }
    
    long start = 1;
    long end = n;
    
    while (start + 1 < end) {
        long mid = start + (end - start) / 2;
        if (((1 + mid) * mid) / 2 <= n) {
            start = mid;            
        } else {
            end = mid;
        }
    }
    
    if (((1 + start) * start) / 2 <= n) {
        return (int)start;
    } else {
        return (int)end;
    }
}
```
