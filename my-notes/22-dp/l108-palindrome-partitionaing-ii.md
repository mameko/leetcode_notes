# L108 Palindrome Partitionaing II

Given a strin&#x67;_&#x73;_, cut\_s\_into some substrings such that every substring is a palindrome.

Return the **minimum** cuts needed for a palindrome partitioning of _s_.

**Example**

Given s =`"aab"`,

Return`1`since the palindrome partitioning \["aa", "b"] could be produced using\_1\_cut.

不知道是哪里抄来的，我把mincut的dp和isPal的dp混在一起写了。有点难理解，下面贴了9章的答案供自己以后参考。

解法一：用一个int的dp数组来记录到这里为止的mincut数目是多少。首先我们进入循环时把min的值初始为i，因为长度为i的string，切i - 1刀，必定能切成全是回文串。（每个串只有一个character）。跟LIS那些一样，首先外层枚举i从0到string的len。每次循环里再用j从0到j找那一段的substring是否回文。如果发现i和j相等，而且i跟j相邻或i跟j中间一段是回文串的话，我们知道i到j这一段是回文，把isPal设成true。然后，我们看看要不要更新现在最小切分刀数。如果j = 0，表示在第一个char，所以返回0。（如果输入的本来已经是一个回文串，eg， a或者bb，我们一刀都不用切，所以0）.如果不是的话，我们看mincut的j - 1，是否用更少的刀数切成回文串。（第j个的下标是j - 1）.

```java
public int minCut(String s) {
    if (s.length() == 0) {
        return 0;
    }

    int[] cut = new int[s.length()];
    boolean isPal[][] = new boolean[s.length()][s.length()];
    for (int i = 1; i < s.length(); i++) {
        int min = i;
        for (int j = 0; j <= i; j++) {
            if (s.charAt(i) == s.charAt(j) && (i <= j + 1 || isPal[i - 1][j + 1])) {
                isPal[i][j] = true;
                min = Math.min(min, j == 0 ? 0 : 1 + cut[j - 1]);
            }
        }
        cut[i] = min;
    }
    return cut[s.length() - 1];
}
```

下面是九章的：首先用n^2求了这个string里哪些部分是palindrome。然后用了LIS里的方法，还是两层循环。感觉比较容易理解。

```java
private boolean[][] getIsPalindrome(String s) {
    boolean[][] isPalindrome = new boolean[s.length()][s.length()];

    for (int i = 0; i < s.length(); i++) {
        isPalindrome[i][i] = true;
    }
    for (int i = 0; i < s.length() - 1; i++) {
        isPalindrome[i][i + 1] = (s.charAt(i) == s.charAt(i + 1));
    }

    for (int length = 2; length < s.length(); length++) {
        for (int start = 0; start + length < s.length(); start++) {
            isPalindrome[start][start + length]
                = isPalindrome[start + 1][start + length - 1] && s.charAt(start) == s.charAt(start + length);
        }
    }

    return isPalindrome;
}

public int minCut(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }

    // preparation
    boolean[][] isPalindrome = getIsPalindrome(s);

    // initialize
    int[] f = new int[s.length() + 1];
    for (int i = 0; i <= s.length(); i++) {
        f[i] = i - 1;
    }

    // main
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
            if (isPalindrome[j][i - 1]) {
                f[i] = Math.min(f[i], f[j] + 1);
            }
        }
    }

    return f[s.length()];
}
```
