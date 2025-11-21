# 516 Longest Palindromic Subsequence

Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.

**Example 1:**\
Input:

```
"bbbab"
```

Output:

```
4
```

One possible longest palindromic subsequence is "bbbb".

**Example 2:**\
Input:

```
"cbbd"
```

Output:

```
2
```

One possible longest palindromic subsequence is "bb".

跟上一题很像，只是这里dp的条件是，如果相等，左下的加2，不等，max（左，下）。因为这里不需要连续，所以如果不等的话，就取用i字母时的最长，或用j字母时中更长的一个就行了。这里用的dp数组是int。T:O(n^2),S:O(n^2)

填右上一半的矩阵

```java
    public int longestPalindromeSubseq(String s) {
        if (s == null || s.isEmpty()) {
            return 0;
        }

        int n = s.length();
        int[][] dp = new int[n][n];

        // init diagonal to 1, length of one char's palindrome is 1
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }

        // fill the matrix for len = 2 -- can omit when len start from 1. add this, len start from 2
//      for (int i = 0; i < n - 1; i++) {
//         dp[i][i + 1] = s.charAt(i) == s.charAt(i + 1) ? 2 : 0;
//     }

        // fill the dp matrix
        for (int len = 1; len < n; len++) {
            for (int start = 0; start + len < n; start++) {
                if (s.charAt(start) == s.charAt(start + len)) {
                    dp[start][start + len] = 2 + dp[start + 1][start + len - 1];
                } else {
                    dp[start][start + len] = Math.max(dp[start + 1][start + len], dp[start][start + len - 1]);
                }
            }
        }

        return dp[0][n - 1];
    }
```

填左下一半的dp。

```java
public int longestPalindromeSubseq(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }

    int n = s.length();
    int[][] dp = new int[n][n];

    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }

    for (int i = 1; i < n; i++) {
        for (int j = i - 1; j >= 0; j--) {
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = 2 + dp[i - 1][j + 1];
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j + 1]);
            }
        }
    }

    return dp[n - 1][0];
}
```
