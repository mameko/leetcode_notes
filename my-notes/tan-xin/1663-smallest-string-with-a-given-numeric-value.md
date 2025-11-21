# 1663 Smallest String With A Given Numeric Value

The **numeric value** of a **lowercase character** is defined as its position `(1-indexed)` in the alphabet, so the numeric value of `a` is `1`, the numeric value of `b` is `2`, the numeric value of `c` is `3`, and so on.

The **numeric value** of a **string** consisting of lowercase characters is defined as the sum of its characters' numeric values. For example, the numeric value of the string `"abe"` is equal to `1 + 2 + 5 = 8`.

You are given two integers `n` and `k`. Return _the **lexicographically smallest string** with **length** equal to `n` and **numeric value** equal to `k`._

Note that a string `x` is lexicographically smaller than string `y` if `x` comes before `y` in dictionary order, that is, either `x` is a prefix of `y`, or if `i` is the first position such that `x[i] != y[i]`, then `x[i]` comes before `y[i]` in alphabetic order.

**Example 1:**

```
Input: n = 3, k = 27
Output: "aay"
Explanation: The numeric value of the string is 1 + 1 + 25 = 27, and it is the smallest string with such a value and length equal to 3.
```

**Example 2:**

```
Input: n = 5, k = 73
Output: "aaszz"
```

**Constraints:**

* `1 <= n <= 105`
* `n <= k <= 26 * n`

这题，一看，不就是[40 Combination Sum II](../3-left-right-tranvesal-max-array-stocks/40-combination-sum-ii.md)的变体吗？这题就是求其中一个方案。但是呢，这里找的是第一个结果，也不好把所有算出来了，然后只返回第一个。想把搜索树截枝，发现不好做。也想了想是不是背包。发现又不像。后来看了答案，原来是贪心。

其实一开始也觉得，能不能从两边入手，找最大/最小的先填了。但因为老师说通常贪心都是不对的，就没往下想。可是...这题还真贪心了。还真从两边的某一边入手。看来有时候还是得变通变通呀。

这题是从后面开始贪心，先找最大的字母填了。T:O(n), S:O(n),sb存了n个。其实这题，让我深思的是，如果来一个follow up，求是否可行，会不会变成DP ？又或者把这题的条件换成k是可以任意大的，如果不可行，返回空串什么的。

```java
public String getSmallestString(int n, int k) {
    if (n < 1 || k < n) {
        return "";
    }

    StringBuilder sb = new StringBuilder();
    int curChar = 26;
    // 没找够字母就继续找
    while (n > 0) {
        // 如果这个字母太大了，挑前面一个
        if (k - curChar < n - 1) {
            curChar--;
            continue;
        }
        // 找到合适字母，加到tmp里
        sb.append((char)('a' + curChar - 1));
        // 少一个字母，n--
        n--;
        // k也得减掉这个字母的值
        k -= curChar;
    }

    // 因为从屁股开始找，然后一直加到后面。所以要反转一下
    return sb.reverse().toString();
}
```
