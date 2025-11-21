# L77 Longest Common Subsequence

Given two strings, find the longest common subsequence (_LCS_).

Your code should return the length o&#x66;_&#x4C;CS_.

**Clarification**

What's the definition of Longest Common Subsequence?

* [https://en.wikipedia.org/wiki/Longest\_common\_subsequence\_problem](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem)
* [http://baike.baidu.com/view/2020307.htm](http://baike.baidu.com/view/2020307.htm)

**Example**

For`"ABCD"`and`"EDCA"`, the\_LCS\_is`"A"`(or`"D"`,`"C"`), return`1`.

For`"ABCD"`and`"EACB"`, the\_LCS\_is`"AC"`, return`2`.

subsequence表示不用连续，这种通过画二维格子来解决。多留一个行一列表示空串。还是初始化第一行第一列为0，因为空串跟string里的char能组成的最长子序列长度为0.（这里其实不用初始化，因为java的int\[]数组初始化为0）.如果现在字串1跟字串2的字幕相等，我们直接把它们前一个状态，就是\[i - 1]\[j - 1]的值加一。如果不相等，我们取max（左边，上边，左上）

```java
public int longestCommonSubsequence(String A, String B) {
    if (A == null || B == null || A.length() == 0 || B.length() == 0) {
        return 0;
    } 

    int collen = A.length();
    int rowlen = B.length();
    int[][] dp = new int[collen + 1][rowlen + 1];

    //init 1st col
    for (int j = 0; j < collen + 1; j++) {
        dp[0][j] = 0;
    }

    //init 1st row
    for (int i = 0; i < rowlen + 1; i++) {
        dp[i][0] = 0;
    }

    for (int i = 1; i < rowlen + 1; i++) {
        for (int j = 1; j < collen + 1; j++) {

            if (A.charAt(i - 1) != B.charAt(j - 1)) {
                dp[i][j] = Math.max(dp[i - 1][j], Math.max(dp[i - 1][j - 1], dp[i][j - 1]));
            } else {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }

        }
    }

    return dp[collen][rowlen];
}
```
