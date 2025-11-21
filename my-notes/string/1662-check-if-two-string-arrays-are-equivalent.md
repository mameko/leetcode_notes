# 1662 Check If Two String Arrays are Equivalent

Given two string arrays `word1` and `word2`, return `true` _if the two arrays **represent** the same string, and_ `false` _otherwise._

A string is **represented** by an array if the array elements concatenated **in order** forms the string.

**Example 1:**

```
Input: word1 = ["ab", "c"], word2 = ["a", "bc"]
Output: true
Explanation:
word1 represents string "ab" + "c" -> "abc"
word2 represents string "a" + "bc" -> "abc"
The strings are the same, so return true.
```

**Example 2:**

```
Input: word1 = ["a", "cb"], word2 = ["ab", "c"]
Output: false
```

**Example 3:**

```
Input: word1  = ["abc", "d", "defg"], word2 = ["abcddefg"]
Output: true
```

**Constraints:**

* `1 <= word1.length, word2.length <= 103`
* `1 <= word1[i].length, word2[i].length <= 103`
* `1 <= sum(word1[i].length), sum(word2[i].length) <= 103`
* `word1[i]` and `word2[i]` consist of lowercase letters.

嘛，就是用指針一邊往后走一邊比較。如果直接能concatenate的話更加容易。第一次寫的時候忘了清空tmp的w1和w2，所以沒過，每次記得清空就ok了

```java
public boolean arrayStringsAreEqual(String[] word1, String[] word2) {
    if ((word1 == null && word2 == null) || (word1.length == 0 && word2.length == 0)) {
        return true;
    } else if (word1 == null || word2 == null) {
        return false;
    }

    int w1Index = 0;
    int w2Index = 0;

    StringBuilder w1 = new StringBuilder();
    StringBuilder w2 = new StringBuilder();

    int i = 0;
    int j = 0;

    while (w1Index < word1.length && w2Index < word2.length) {
        w1 = w1.append(word1[w1Index]);
        w2 = w2.append(word2[w2Index]);

        while (i < w1.length() && j < w2.length()) {
            if (w1.charAt(i) != w2.charAt(j)) {
                return false;
            }
            i++;
            j++;
        }

        if (i == w1.length()) {
            w1Index++;
            i = 0;
        }

        if (j == w2.length()) {
            w2Index++;
            j = 0;
        }
        w1 = new StringBuilder();
        w2 = new StringBuilder();
    }
    return w1Index == word1.length && w2Index == word2.length;
}
```
