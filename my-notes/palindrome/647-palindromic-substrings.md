# 647 Palindromic Substrings

Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

**Example 1:**

```
Input: "abc"
Output: 3

Explanation: Three palindromic strings: "a", "b", "c".
```

**Example 2:**

```
Input: "aaa"
Output: 6

Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

**Note:**

1. The input string length won't exceed 1000.

这题跟5 Longest Palindromic substring的做法基本一样，还是用2维dp来check哪些substring是palindrome。这题更简单，不用算longest，直接数数目。

```java
public int countSubstrings(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }

    int len = s.length();
    boolean[][] isPar = new boolean[len][len];
    int cnt = 0;

    for (int i = 0; i < len; i++) {
        isPar[i][i] = true;
    }

    for (int i = 0; i < len; i++) {
        for (int j = i; j >= 0; j--) {
            if (s.charAt(i) == s.charAt(j) && (i - j <= 1 || isPar[i - 1][j + 1])) {
                cnt++;
                isPar[i][j] = true;
            }
        }
    }

    return cnt;
}
```
