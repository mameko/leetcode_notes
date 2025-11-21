# 5 Longest Palindromic Substring

Given a string**s**, find the longest palindromic substring in**s**. You may assume that the maximum length of **s** is 1000.

**Example:**

```
Input: "babad"
Output: "bab"

Note: "aba" is also a valid answer.
```

**Example:**

```
Input: "cbbd"
Output: "bb"
```

6年后再上九章，发现有个新的算法。这题的套路可以如下，一开始，给n^3的。然后再给出中间字符枚举，n^2的。最后再给动规，也是n^2，但听起来比较厉害。

中间枚举的方法是，我们可以假设每个字符可能是回文的中间字符。分为两种情况，一种是odd的回文串，那么中心那里只有1个字母。譬如，aba，以b为中心；even的呢，中间就是两个。abba，以bb为中心向两边找最长。

这里我们写了一个函数做这个处理，函数比较时要注意越界。找到最长的，就substring返回。因为break的条件符合的时候，已经不是回文了，所以left要+1。right不用-1是因为java substring的第二个parameter是不包括的\[start, end)

```java
public String longestPalindrome(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }

    String longest = "";
    for (int i = 0; i < s.length(); i++) {
        String oddParStr = getLongestParFrom(s, i, i);
        if (oddParStr.length() > longest.length()) {
            longest = oddParStr;
        }

        String evenParStr = getLongestParFrom(s, i, i + 1);
        if (evenParStr.length() > longest.length()) {
            longest = evenParStr;
        }
    }

    return longest;
}

private String getLongestParFrom(String str, int left, int right) {
    while (left >= 0 && right < str.length()) {
        if (str.charAt(left) != str.charAt(right)) {
            break;
        }

        left--;
        right++;
    }

    return str.substring(left + 1, right);
}
```



可以用九章的dp，也可以用不知道是谁写的dp。

一步到位的dp：

```java
public String longestPalindrome(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }

    int n = s.length();
    boolean[][] isPal = new boolean[n][n];

    int maxLen = 0;
    String res = "";
    for (int i = 0; i < n; i++) {
        for (int j = 0; j <= i; j++) {
            // i - j <= 1, same char or next to each other
            isPal[i][j] = s.charAt(i) == s.charAt(j) && (i - j <= 1 || isPal[i - 1][j + 1]); 
            if (isPal[i][j] && maxLen < i - j + 1) {
                maxLen = i - j + 1;
                res = s.substring(j, i + 1);
            }
        }
    }

    return res;
}
```

分步填的dp：

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() == 0) {
        return "";
    }

    int len = s.length();
    boolean[][] isPar = new boolean[len][len];
    calParMatrix(isPar, s);
    int max = 0;
    String res = "";
    for (int i = 0; i < len; i++) {
        for (int j = i; j < len; j++) {
            if (isPar[i][j]) {
                int cur = j - i + 1;
                if (max < cur) {
                    max = cur;
                    res = s.substring(i, j + 1);
                }
            }
        }
    }
    return res;
}

private void calParMatrix(boolean[][] isPar, String s) {
    int len = isPar.length;

    // diagonal are all true
    for (int i = 0; i < len; i++) {
        isPar[i][i] = true;
    }

    // chars next to each other
    for (int i = 0; i < len - 1; i++) {
        isPar[i][i + 1] = (s.charAt(i) == s.charAt(i + 1));
    }

    // find the rest
    for (int l = 2; l < len; l++) {
        for (int start = 0; start + l < len; start++) {
            isPar[start][start + l] = s.charAt(start) == s.charAt(start + l) && isPar[start + 1][start + l - 1];
        }
    }
}
```
