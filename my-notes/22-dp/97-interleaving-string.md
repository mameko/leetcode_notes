# 97 Interleaving String

Given three strings:_s1_,_s2_,_s3_, determine whethe&#x72;_&#x73;3\_is formed by the interleaving of\_s1\_and\_s2_.

**Example**

For s1 =`"aabcc"`, s2 =`"dbbca"`

* When s3 =`"aadbbcbcac"`, return`true`.
* When s3 =`"aadbbbaccc"`, return`false`.

[**Challenge**](http://www.lintcode.com/en/problem/interleaving-string/#challenge)

O(n2) time or better

这题还是双序列型。\[0，0]初始化为true（空串match s3的第0个也是空串），首先初始化第一列和第一行。如果s3的字母跟s2或s1的字母相同，然后前一个是match的话，我们设为true（相当于只用s1或s2来match s3）。填完两边然后填中间，如果s3的字母跟s1或s2的相等，我们判断前一个字母是否能够组成s3，如果是把当前设为true。

```java
public boolean isInterleave(String s1, String s2, String s3) {
    if (s1 == null || s2 == null || s3 == null) {
        return false;
    }

    if (s3.length() != s1.length() + s2.length()) {
        return false;
    }

    boolean[][] dp = new boolean[s2.length() + 1][s1.length() + 1];
    dp[0][0] = true;

    //init 1st col
    for (int i = 1; i <= s2.length(); i++) {
        dp[i][0] = (s3.charAt(i - 1) == s2.charAt(i - 1)) && dp[i - 1][0];
    }

    //init 1st row
    for (int j = 1; j <= s1.length(); j++) {
        dp[0][j] = (s3.charAt(j - 1) == s1.charAt(j - 1)) && dp[0][j - 1];
    }

    for (int i = 1; i <= s2.length(); i++) {
        for (int j = 1; j <= s1.length(); j++) {
            if (((s3.charAt(i + j - 1) == s1.charAt(j - 1)) && dp[i][j - 1])
                ||((s3.charAt(i + j - 1) == s2.charAt(i - 1)) && dp[i - 1][j])) {
                    dp[i][j] = true;
                }
        }
    }

    return dp[s2.length()][s1.length()];
}
```
