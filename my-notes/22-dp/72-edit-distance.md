# 72 Edit Distance

Given two word&#x73;_&#x77;ord1\_and\_word2_, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

* Insert a character
* Delete a character
* Replace a character

**Example**

Given word1 =`"mart"`and word2 =`"karma"`, return`3`.

这题还是建立一个二维的dp数组，多加一行一列给空串。初始化为下标，因为空串到现有字符串的edit distance就是现在这个串的长度。（eg.从空串变成abc，需要3个edit）当字母相等时，我们可以不用增加edit distance，把dp\[i]\[j]赋值为左上角的值。如果我们发现不一样，证明我们需要改一下，我们取min（左边，上边，左上）+ 1。左边表示去掉一个，上边表示插入一个，左上表示改一个。

```java
public int minDistance(String word1, String word2) {
    if (word1 == null && word2 == null) {
        return 0;
    } else if (word1.length() == 0 && word2.length() == 0) {
        return 0;
    } else if (word1.length() == 0 && word2.length() > 0) {
        return word2.length();
    } else if (word1.length() > 0 && word2.length() == 0) {
        return word1.length();
    }

    int len1 = word1.length();
    int len2 = word2.length();

    int[][] dp = new int[len1 + 1][len2 + 1];

    // init 1st row
    for (int i = 0; i < len1 + 1; i++) {
        dp[i][0] = i;
    }

    // init 1st col
    for (int j = 0; j < len2 + 1; j++) {
        dp[0][j] = j;
    }

        //dp[i][j] = dp[i - 1][j - 1] when char equals
        //else equals min(up, left, upleft) + 1
    for (int i = 1; i < len1 + 1; i++) {
        for (int j = 1; j < len2 + 1; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j], Math.min(dp[i][j - 1], dp[i - 1][j - 1])) + 1;
            }
        }
    }

    return dp[len1][len2];
}
```
