# 115 Distinct Subsequences

Given a string**S**and a string**T**, count the number of distinct subsequences of**T**in**S**.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie,`"ACE"`is a subsequence of`"ABCDE"`while`"AEC"`is not).

**Example**

Given S =`"rabbbit"`, T =`"rabbit"`, return`3`.

[**Challenge**](http://www.lintcode.com/en/problem/distinct-subsequences/#challenge)

Do it in O(n2) time and O(n) memory.

O(n2) memory is also acceptable if you do not know how to optimize memory.

这题还是加一行加一列表示空串，初始化第一列为1，因为空串是现有字符串的一个字串。每次我们比较两个串里的字符，如果相等，我们把不算这两个字母的数目\[i - 1]\[j - 1]加上从source里去掉这个字母的数目\[i]\[j - 1]。如果不相等的话，我们直接把dp\[i]\[j]赋值为从source里去掉这个字母的数目\[i]\[j - 1]。

```java
public int numDistinct(String S, String T) {
    if ((S == null && T == null) || (S.length() == 0 && T.length() == 0)) {
        return 1;
    }

    if (S.length() < T.length()) {
        return 0;
    } 

    int[][] dp = new int[T.length() + 1][S.length() + 1];
    for (int i = 0; i <= S.length(); i++) {
        dp[0][i] = 1;
    }
    for (int i = 1; i <= T.length(); i++) {
        for (int j = i; j <= S.length(); j++) {
            if (T.charAt(i - 1) == S.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + dp[i][j - 1];
            } else {
                dp[i][j] = dp[i][j - 1];
            }
        }
    }
    return dp[T.length()][S.length()];
}
```
