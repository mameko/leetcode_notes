# 336 Palindrome Pairs

You are given a **0-indexed** array of **unique** strings `words`.

A **palindrome pair** is a pair of integers `(i, j)` such that:

* `0 <= i, j < word.length`,
* `i != j`, and
* `words[i] + words[j]` (the concatenation of the two strings) is a palindrome.

Return _an array of all the **palindrome pairs** of_ `words`.

&#x20;

**Example 1:**

<pre><code><strong>Input: words = ["abcd","dcba","lls","s","sssll"]
</strong><strong>Output: [[0,1],[1,0],[3,2],[2,4]]
</strong><strong>Explanation: The palindromes are ["abcddcba","dcbaabcd","slls","llssssll"]
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: words = ["bat","tab","cat"]
</strong><strong>Output: [[0,1],[1,0]]
</strong><strong>Explanation: The palindromes are ["battab","tabbat"]
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: words = ["a",""]
</strong><strong>Output: [[0,1],[1,0]]
</strong><strong>Explanation: The palindromes are ["a","a"]
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= words.length <= 5000`
* `0 <= words[i].length <= 300`
* `words[i]` consists of lowercase English letters.

这题呢，超级难，bruteforce呢，是T:O(n^2 \* k)。

然后呢，看了答案，通过把前缀和后缀存入hashMap中优化，能优化到T:O(k^2 \* n)，但n超级大的时候，会比brute force好。这里有3种情况需要加到result里：

1.存在reverse，譬如，list里有CAT和TAC。那么这对可以加

2.word1比较短，word2比较长，word2后缀是word1的reverse而且word2中间是palindrome。譬如，CAT，**LOLTAC**

3.word1比较长，word2比较短，word1前缀是word2的reverse而且word1中间是palindrome，譬如，**CATLOL**，TAC

我们每遇到一个word，就算一次prefix和suffix（剩余部分是valid palindrome），然后reverse这个prefix和suffix看是不是存在在worlist里。如果是，那么这两个可以组成一对。

还有一种解法是用Trie，因为我们这里算了一堆prefix，suffix。具体做法要看solution，但其实时间复杂度没变。听说是如果我们要support变增加，变查找的功能时候，Trie比较好。

<pre class="language-java"><code class="lang-java"><strong>// 抄了一遍
</strong>public List&#x3C;List&#x3C;Integer>> palindromePairs(String[] words) {
    if (words == null || words.length == 0) {
        return null;
    }

    Map&#x3C;String, Integer> wordMap = new HashMap&#x3C;>();
    int len = words.length;
    for (int i = 0; i &#x3C; len; i++) {
        wordMap.put(words[i], i);
    }

    List&#x3C;List&#x3C;Integer>> result = new ArrayList&#x3C;>();
    for (String word : wordMap.keySet()) {
        int curInd = wordMap.get(word);
        String reverseWord = new StringBuilder(word).reverse().toString();

        // case 1, reverse of this word exist
        if (wordMap.containsKey(reverseWord) &#x26;&#x26; wordMap.get(reverseWord) != curInd) {
            result.add(Arrays.asList(curInd, wordMap.get(reverseWord)));
        }

        // case 2, suffix, current word is 2nd word
        for (String suffix : getAllValidSuffix(word)) {
            String reverseSuffix = new StringBuilder(suffix).reverse().toString();
            if (wordMap.containsKey(reverseSuffix)) {
                result.add(Arrays.asList(wordMap.get(reverseSuffix), curInd));
            }
        }

        // case 3, prefix, current word is 1st word
        for (String prefix : getAllValidPrefix(word)) {
            String reversePrefix = new StringBuilder(prefix).reverse().toString();
            if (wordMap.containsKey(reversePrefix)) {
                result.add(Arrays.asList(curInd, wordMap.get(reversePrefix)));
            }
        }
    }

    return result;
}

private List&#x3C;String> getAllValidSuffix(String word) {
    List&#x3C;String> suffixes = new ArrayList&#x3C;>();
    for (int i = 0; i &#x3C; word.length(); i++) {
        // 前面是valid palindrome
        if (isPalindrome(word, 0, i)) {
            // 把后半部分放入suffix list里
            suffixes.add(word.substring(i + 1, word.length()));//houmia
        }
    }
    return suffixes;
}

private List&#x3C;String> getAllValidPrefix(String word) {
    List&#x3C;String> prefixes = new ArrayList&#x3C;>();
    for (int i = 0; i &#x3C; word.length(); i++) {
        // 后面是valid palindrome
        if (isPalindrome(word, i, word.length() - 1)) {
            // 把前半部分放入prefix list里
            prefixes.add(word.substring(0, i));
        }
    }
    return prefixes;
}

private boolean isPalindrome(String str, int start, int end) {
    for (int i = start, j = end; i &#x3C; j; i++, j--) {
        if (str.charAt(i) != str.charAt(j)) {
            return false;
        }
    }
    return true;
}

<strong>
</strong><strong>// 暴力算法
</strong>public List&#x3C;List&#x3C;Integer>> palindromePairs(String[] words) {
    if (words == null || words.length == 0) {
        return null;
    }

    List&#x3C;List&#x3C;Integer>> result = new ArrayList&#x3C;>();
    int len = words.length;
    for (int i = 0; i &#x3C; len; i++) {
        for (int j = 0; j &#x3C; len; j++) {
            if (i == j) {
                continue;
            }

            String combinedStr = words[i] + words[j];
            if (isPalindrome(combinedStr)) {
                List&#x3C;Integer> oneAns = Arrays.asList(i, j);
                result.add(oneAns);
            }
        }
    }
    return result;
}

private boolean isPalindrome(String str) {
    for (int i = 0, j = str.length() - 1; i &#x3C; j; i++, j--) {
        if (str.charAt(i) != str.charAt(j)) {
            return false;
        }
    }
    return true;
}
</code></pre>
