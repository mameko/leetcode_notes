# 953 Verifying an Alien Dictionary

In an alien language, surprisingly they also use english lowercase letters, but possibly in a different `order`. The `order` of the alphabet is some permutation of lowercase letters.

Given a sequence of `words` written in the alien language, and the `order` of the alphabet, return `true` if and only if the given `words` are sorted lexicographicaly in this alien language.

**Example 1:**

```
Input: words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
Output: true
Explanation: As 'h' comes before 'l' in this language, then the sequence is sorted.
```

**Example 2:**

```
Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.
```

**Example 3:**

```
Input: words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
Output: false
Explanation: The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" > "app", because 'l' > '∅', where '∅' is defined as the blank character which is less than any other character (More info).
```

**Constraints:**

* `1 <= words.length <= 100`
* `1 <= words[i].length <= 20`
* `order.length == 26`
* All characters in `words[i]` and `order` are English lowercase letters.

这题就是考str comparison。一个一个比较跟把两个拿出来比较一样，所以多写了一个函数，不容易出错。T:O(M + N), M是words里所有字符的总数，N是字母表的大小，因为都要过一遍。S:O(N)，其实这里以为只有26个字母，所以我们也可以说成T:O(M), S:O(1）

```java
public boolean isAlienSorted(String[] words, String order) {
    if (words == null || order == null || words.length == 0 || order.isEmpty()) {
        return false;
    }

    if (words.length == 0) {
        return true;
    }

    Map<Character, Integer> charToLoc = new HashMap<>();
    for (int i = 0; i < order.length(); i++) {
        charToLoc.put(order.charAt(i), i);
    }

    for (int i = 1; i < words.length; i++) {
        if (!isTwoOrdered(words[i - 1], words[i], charToLoc)) {
            return false;
        }
    }

    return true;
}

private boolean isTwoOrdered(String word1, String word2, Map<Character, Integer> charToLoc) {
    int i = 0;
    int j = 0;

    while (i < word1.length() && j < word2.length()) {
        char one = word1.charAt(i);
        char two = word2.charAt(j);
        if (charToLoc.get(one) < charToLoc.get(two)) {
            return true;
        } else if (charToLoc.get(one) > charToLoc.get(two)) {
            return false;
        }

        i++;
        j++;
    }

    if (word2.length() < word1.length()) {
        return false;
    }

    return true;
}
```
