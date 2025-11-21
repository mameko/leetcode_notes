# 139 Word Break

Given a**non-empty**stringsand a dictionarywordDictcontaining a list of**non-empty**words, determine ifscan be segmented into a space-separated sequence of one or more dictionary words. You may assume the dictionary does not contain duplicate words.

For example, given\
s=`"leetcode"`,\
dict=`["leet", "code"]`.

Return true because`"leetcode"`can be segmented as`"leet code"`.

这题问的是可不可行，所以用了一个boolean的dp数组。因为是序列型的，所以多留一个空位表示前0个。初始化为true（空串）。如果word能break到空串证明能完全切分。然后每次从i往前找，如果切分出来的单词能在字典里找到，而且切完之后前面一截也是能完全切分的，我们把现在这个i设成true。T:O(n^2)，S:O(n)

```java
public boolean wordBreak(String s, List<String> wordDict) {
    if (wordDict == null || wordDict.size() == 0) {
        return false;
    }

    int n = s.length();
    boolean[] dp = new boolean[n + 1];
    dp[0] = true;
    for (int i = 1; i <= n; i++) {
        for (int j = i - 1; j >= 0; j--) {
            String sub = s.substring(j, i);
            if (wordDict.contains(sub) && dp[j]) {
                dp[i] = true;
                break;
            }
        }
    }

    return dp[n];
}
```
