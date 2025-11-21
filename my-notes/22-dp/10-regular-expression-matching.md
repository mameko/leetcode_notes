# 10 Regular Expression Matching

Implement regular expression matching with support for`'.'`and`'*'`.

```
'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the 
entire
 input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```

序列型，还是得多加一行一列表示空串。初始化空空（0， 0）为true。然后把第一行初始化了，如果看到\*我们看前2个位置，如果match的话（用0个字母\*来match），我们设为true。然后处理中间部分。同样，如果相等或者能用.(点)来match一个字母的话，我们看左上角，（把这两个去掉以后）是否match。如果遇到\*,同样看前两个，如果那个地方是ture的话，这个格子是true。如果那个地方是false，我们可以看j - 2是否等于.(点）或i - 1.如果是的话，可以看上面一个是否true，是的话就设成true。注意，这里需要输入的pattern是合法的，不然会有out of bound。例如：pattern就是一个"\*"，这时j - 2会越界。T:O(mn), S:O(mn)

```java
public boolean isMatch(String s, String p) {
    if (s == null || p == null) {
        return false;
    }

    int n = s.length();
    int m = p.length();
    boolean[][] dp = new boolean[n + 1][m + 1];
    dp[0][0] = true;

    // init the 1st row
    for (int i = 1; i <= m; i++) {
        if (p.charAt(i - 1) == '*') {
            dp[0][i] = dp[0][i - 2];
        }
    }

    // fill in the middle part
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            if (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.') {
                dp[i][j] = dp[i - 1][j - 1];
            } else if (p.charAt(j - 1) == '*') {
                dp[i][j] = dp[i][j - 2];
                if (p.charAt(j - 2) == s.charAt(i - 1) || p.charAt(j - 2) == '.') {
                    dp[i][j] = dp[i - 1][j] || dp[i][j];
                }
            }
        }
    }

    return dp[n][m];
}
```
