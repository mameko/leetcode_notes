# 172 Factorial Trailing Zeroes

Given an integern, return the number of trailing zeroes inn!.

**Example 1:**

```
Input: 3
Output: 0

Explanation: 3! = 6, no trailing zero.
```

**Example 2:**

```
Input: 5
Output: 1

Explanation: 5! = 120, one trailing zero.
```

**Note:**&#x59;our solution should be in logarithmic time complexity.

这题cc189有然后两年前做过。但感觉纯数学就一直没写，但看到在软软的面经里，还是写写吧。因为要找0的数目，那么就是找10的数目，10 = 2 X 5, 而且2的数目比5多，所以我们其实是在数这个n里5的数目。

```java
public int trailingZeroes(int n) {
    if (n < 0) {
        return 0;
    }

    int res = 0;
    while (n > 0) {
        res += n / 5;
        n = n / 5;
    }

    return res;
}
```
