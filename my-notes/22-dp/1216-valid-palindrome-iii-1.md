# 1216 Valid Palindrome III

Given a string `s` and an integer `k`, find out if the given string is a _K-Palindrome_ or not.

A string is K-Palindrome if it can be transformed into a palindrome by removing at most `k` characters from it.

**Example 1:**

```
Input: s = "abcdeca", k = 2
Output: true
Explanation: Remove 'b' and 'e' characters.
```

**Constraints:**

* `1 <= s.length <= 1000`
* `s` has only lowercase English letters.
* `1 <= k <= s.length`

这题一开始的想法是运用[680 Valid Palindrome II](../two-pointers-other/680-valid-palindrome-ii.md) 递归的方法继续下去。譬如，我们只能去掉左边，去掉右边，或者两边都去掉。最后如果3个之中有一个可行的，就return true。但倒了47/50的地方，有一个test case过不了...还以为自己神功附体，突然写出来了hard题，而且还是memorization的写...是我想多了...（在leetCodeJan-string里）

正确的打开方式是，用DP。跟[72 Edit Distance](72-edit-distance.md)和[516 Longest Palindromic Subsequence](../palindrome/516-longest-palindromic-subsequence.md) 相似。其实我的解法，差了一步，就是去掉左or右的时候，没有取最小值。

```java
Integer memo[][];
public boolean isValidPalindrome(String s, int k) {
    if (s == null || s.length() == 0 || k < 0) {
        return false;
    }

    memo = new Integer[s.length()][s.length()];
    int parEditDistance = editDistanceForPar(s, 0, s.length() - 1);

    return parEditDistance <= k;
}

private int editDistanceForPar(String s, int i, int j) {
    // single char， no edit
    if (i == j) { 
        return 0;
    }
    // two chars, if equal no edit, if not equal edit 1
    if (i == j - 1) { 
        return s.charAt(i) == s.charAt(j) ? 0 : 1;
    }
    // after base cases, check if we've already computed this range
    if (memo[i][j] != null) { 
        return memo[i][j];
    }

    // if 2 char equals, we check the content in between
    if (s.charAt(i) == s.charAt(j)) {
        return editDistanceForPar(s, i + 1, j - 1);
    }

    // if 2 chars are not equal, 
    // we removeLeft or removeRight, then check the contents
    int removeLeft = editDistanceForPar(s, i + 1, j);
    int removeRight = editDistanceForPar(s, i, j - 1);

    // at the end, we would like to return the smallest edit distance
    // use memo[][] to remember what we did
    memo[i][j] = 1 + Math.min(removeLeft, removeRight); 
    return 1 + Math.min(removeLeft, removeRight);
}
```
