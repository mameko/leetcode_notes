# 44 Wildcard matching

Implement wildcard pattern matching with support for`'?'`and`'*'`.

```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the 
entire
 input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

这题用dp来做，虽然也看到其他方法，也只有写dp的时间了。这里有3个地方需要注意。1，因为是序列型，dp矩阵多加一行一列表示空串；2，初始化的时候，要注意只有‘\*'能配上空串，配上之后就是等于把这一格遮住能match就match，不能match就不能match。3，在给中间的赋值时，当我们遇到‘\*'时，不用loop一次前一列看有没有match（把\*匹配多个），我们可以这样看，如果不用\*的时候，我们能match或者在用\* match这一个字符，两者之中只要有一个是true就表示这个串到此为止都能match。

```java
public boolean isMatch(String s, String p) {
    if (s == null || p == null) {
        return false;
    }

    int n = s.length();
    int m = p.length();

    boolean[][] dp = new boolean[n + 1][m + 1];
    dp[0][0] = true;// empty string matach empty pattern

    // init first row
    for (int i = 1; i <= m; i++) {
        if (p.charAt(i - 1) == '*') {
            dp[0][i] = dp[0][i - 1];
        }
    }

    // fill in the rest of dp
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '?') {
                dp[i][j] = dp[i - 1][j - 1];
            } else if (p.charAt(j - 1) == '*') {
                dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
            }
        }
    }

    return dp[n][m];
}
```
