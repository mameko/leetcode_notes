# 425 Word Squares

Given a set of word&#x73;**(without duplicates)**, find all[word squares](https://en.wikipedia.org/wiki/Word_square)you can build from them.

A sequence of words forms a valid word square if thekthrow and column read the exact same string, where 0 ≤k< max(numRows, numColumns).

For example, the word sequence`["ball","area","lead","lady"]`forms a word square because each word reads the same both horizontally and vertically.

```
b a l l
a r e a
l e a d
l a d y
```

**Note:**

1. There are at least 1 and at most 1000 words.
2. All words will have the exact same length.
3. Word length is at least 1 and at most 5.
4. Each word contains only lowercase English alphabet`a-z`.

**Example 1:**

```
Input:

["area","lead","wall","lady","ball"]


Output:

[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]


Explanation:

The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

**Example 2:**

```
Input:

["abat","baba","atan","atal"]


Output:

[
  [ "baba",
    "abat",
    "baba",
    "atan"
  ],
  [ "baba",
    "abat",
    "baba",
    "atal"
  ]
]


Explanation:

The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```

这题求所以方案数，用深搜。但如果不剪枝的话会TLE的。这题可以用hashmap或者trie来装前缀。用前缀来剪。因为如果你加了新单词之后，没有这个前缀的词语的话，证明做不下去，我们就可以return了。例如填完第二行，发现没有le开头的词，我们就可以退出了，因为继续不下去，退出行一层换一个词再试

```java
public List<List<String>> wordSquares(String[] words) {
    List<List<String>> res = new ArrayList<>();
    if (words == null || words.length == 0) {
        return res;
    }

    Map<String, List<String>> pre = new HashMap<>();
    init(words, pre, words[0].length());

    List<String> tmp = new ArrayList<>();
    dfs(words, res, tmp, pre, 0, words[0].length());

    return res;
}

private void init(String[] words, Map<String, List<String>> pre, int len) {
    for (int i = 0; i < words.length; i++) {
        pre.putIfAbsent("", new ArrayList<>());// 注意“”表示所有单词都加进去。一开始时是空串我们所有单词都得试一试
        pre.get("").add(words[i]);
        for (int j = 0; j < len; j++) {
            String prefix = words[i].substring(0, j + 1);
            pre.putIfAbsent(prefix, new ArrayList<>());
            pre.get(prefix).add(words[i]);
        }
    }
}

private void dfs(String[] words, List<List<String>> res, List<String> tmp, Map<String, List<String>> pre, int row,
        int len) {
    if (row == len) { // 我们找够单词了，所以加进结果
        res.add(new ArrayList<>(tmp));
        return;
    }
    // 找出前缀来剪枝
    StringBuilder prefix = new StringBuilder();
    for (int i = 0; i < tmp.size(); i++) {
        prefix.append(tmp.get(i).charAt(row));
    }
    // 如果我们没找到以这前缀开头的，我们返回，不继续找下去了。
    List<String> wo = pre.get(prefix.toString());
    if (wo == null) {
        return;
    }
    // 只有符合前缀的那些词能继续下去，其他的不用看。
    for (String word : wo) {
        tmp.add(word);
        dfs(words, res, tmp, pre, row + 1, len);
        tmp.remove(tmp.size() - 1);
    }
}
```
