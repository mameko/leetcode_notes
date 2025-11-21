# L581 Longest Repeating Subsequence

Given a string, find length of the longest repeating subsequence such that the two subsequence don’t have same string character at same position, i.e., any`ith`character in the two subsequences shouldn’t have the same index in the original string.

**Example**

str =`abc`, return`0`, There is no repeating subsequence

str =`aab`, return`1`, The two subsequence are`a`(first) and`a`(second).\
Note that`b`cannot be considered as part of subsequence as it would be at same index in both.

str =`aabb`, return`2`

这题还是双序列型。我们从i = 1 开始沿着对角线填好右上半的三角形。填的方法是，如果发现相等但不是同样下标的字母，我们把去掉这个字母的长度加1，左上+1.如果不等的话，把不要i这个字母，或者不要j这个字母时最长的付给现在这个cell。

```java
  public int longestRepeatingSubsequence(String str) {
        if (str == null || str.length() == 0) {
            return 0;
        }

        char[] cs = str.toCharArray();
        int len = cs.length;
        int[][] dp = new int[len + 1][len + 1];

        for (int i = 1; i <= len; i++) {
            for (int j = i; j <= len; j++) {
                if (cs[i - 1] == cs[j - 1] && i != j) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }

        }

        return dp[len][len];
    }
```
