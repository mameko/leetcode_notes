# 87 Scramble String

Given a string`s1`, we may represent it as a binary tree by partitioning it to two non-empty substrings recursively.

Below is one possible representation of`s1 = "great"`:

```
    great
   /    \
  gr    eat
 / \    /  \
g   r  e   at
           / \
          a   t
```

To scramble the string, we may choose any non-leaf node and swap its two children.

For example, if we choose the node`"gr"`and swap its two children, it produces a scrambled string`"rgeat"`.

```
    rgeat
   /    \
  rg    eat
 / \    /  \
r   g  e   at
           / \
          a   t
```

We say that`"rgeat"`is a scrambled string of`"great"`.

Similarly, if we continue to swap the children of nodes`"eat"`and`"at"`, it produces a scrambled string`"rgtae"`.

```
    rgtae
   /    \
  rg    tae
 / \    /  \
r   g  ta  e
       / \
      t   a
```

We say that`"rgtae"`is a scrambled string of`"great"`.

Given two strings`s1`and`s2`of the same length, determine if`s2`is a scrambled string of`s1`.

[**Challenge**](http://www.lintcode.com/en/problem/scramble-string/#challenge)

O(n4) time？总共有n3个状态，每个转换要n的复杂度（看递推的解法）

brute force + 剪枝？

```java
public boolean isScramble(String s1, String s2) {
    // if len is different, they won't be scrable of each other
    if (s1.length() != s2.length()) {
        return false;
    }

    // if both are empty string, or they equal to each other, they are scrable string
    if (s1.isEmpty() || s1.equals(s2)) {
        return true;
    }

    // cut down the compare when 2 string doesn't have same amount of chars
    if (!isValid(s1, s2)) {
        return false;
    }

    // now try to cut both strings at each of location between 1 to len - 1
    int len = s1.length();
    for (int i = 1; i < len; i++) {
        String s11 = s1.substring(0, i);
        String s12 = s1.substring(i, len);

        String s21 = s2.substring(0, i);
        String s22 = s2.substring(i, len);
        String s23 = s2.substring(0, len - i);
        String s24 = s2.substring(len - i, len);

        if (isScramble(s11, s21) && isScramble(s12, s22)) {
            return true;
        }

        if (isScramble(s11, s24) && isScramble(s12, s23)) {
            return true;
        }

    }
    return false;
}

private boolean isValid(String s1, String s2) {
    char[] c1 = s1.toCharArray();
    char[] c2 = s2.toCharArray();
    Arrays.sort(c1);
    Arrays.sort(c2);
    String sorted1 = new String(c1);
    String sorted2 = new String(c2);

    if (!sorted1.equals(sorted2)) {
        return false;
    }

    return true;
}
```

记忆化搜：

```java
HashMap<String, Boolean> hash = new HashMap<String, Boolean>();

public boolean isScramble(String s1, String s2) {
    // Write your code here
    if (s1.length() != s2.length())
        return false;

    if (hash.containsKey(s1 + "#" + s2))
        return hash.get(s1 + "#" + s2);

    int n = s1.length();
    if (n == 1) {
        return s1.charAt(0) == s2.charAt(0);
    }
    for (int k = 1; k < n; ++k) {
        if (isScramble(s1.substring(0, k), s2.substring(0, k)) &&
            isScramble(s1.substring(k, n), s2.substring(k, n)) ||
            isScramble(s1.substring(0, k), s2.substring(n - k, n)) &&
            isScramble(s1.substring(k, n), s2.substring(0, n - k))
            ) {
            hash.put(s1 + "#" + s2, true);
            return true;
        }
    }
    hash.put(s1 + "#" + s2, false);
    return false;
}
```

递推：

```java
public boolean isScramble(String s1, String s2) {
    // Write your code here
    if (s1.length() != s2.length())
        return false;
    int n = s1.length();
    boolean[][][] dp = new boolean[n][n][n+1];
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j)
            dp[i][j][1] = s1.charAt(i) == s2.charAt(j);

    for (int len = 2; len <= n; ++len)
        for (int x = 0; x < n && x + len <= n; ++x)
            for (int y = 0; y < n && y + len <=n; ++y)
                for (int k= 1; k < len; ++k)
                dp[x][y][len] |= dp[x][y][k] && dp[x + k][y + k][len - k]
                || dp[x][y + len - k][k] && dp[x + k][y][len - k];

    return dp[0][0][n];
}
```
