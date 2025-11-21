# 140 Word Break II

Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

**Example**

Gieve s =`lintcode`,\
dict =`["de", "ding", "co", "code", "lint"]`.

A solution is`["lint code", "lint co de"]`.

最近上高频班有好像学了一种新的理解方式。其实也是记忆化搜索，就是弄一个hashmap来存结果。每次把字符串一格格剁开。找到了dict里的前缀之后，开始递归找下面的。每次返回一条list。把后面分好以后，在这一层组装好答案(把这一层找到的左半部份贴上去。

```java
public List<String> wordBreak(String s, Set<String> wordDict) {
    if (s == null || s.length() == 0 || wordDict == null || wordDict.size() == 0) {
        return new ArrayList<>();
    }

    Map<String, List<String>> memo = new HashMap<>();
    memo.putIfAbsent("", new ArrayList<>()); // 初始情况，空字符串，    
    memo.get("").add("");                    // 分到底了没得分时返回的。
    return dfs(memo, wordDict, s);
}

private List<String> dfs(Map<String, List<String>> memo,
                         Set<String> wordDict,
                         String s) {
    if (memo.containsKey(s)) { // 如果我们已经找过了，直接返回那段list
        return memo.get(s);
    }

    List<String> res = new ArrayList<>(); // 用来装这一层的结果
    for (int len = 1; len <= s.length(); len++) { // 一格格地切
        String left = s.substring(0, len); // 切成两半
        String right = s.substring(len);

        if (wordDict.contains(left)) { // 切到左边是字典里的词之后
            List<String> rest = dfs(memo, wordDict, right); //递归找后面的，这个递归完就返回了后半部分的分法
            for (String str : rest) { // 组装好这一层的结果。
                if (str == "") {      // 每个前都加一个 left
                    res.add(left);    // 这里是为了避免多加空格所以分开了。
                } else {              // 例如：“lint” 而不是“lint ”
                    res.add(left + " " + str);
                }
            }
        }
    }

    memo.put(s, res);// 放到公告板上
    return res; // 返回这一层的结果
}
```

这题因为要返回所有结果，所以是搜索。我们可以brute force地搜。搜到了就加结果。但那样的话复杂度很高，指数级的O(2^n)。所以我们通过用一个boolean数组记录不能break的来减低复杂度了。递归里还是backtrack着来做。在backtrack的时候顺便填那个剪枝的boolean数组。如果递归以后，我们发现结果数组没有变长，证明这次递归地break到最后并不可行，所以我们把dp里的那个值设成false。（dp全部初始为true，只是找到不行的时候我们才记住这个地方不行。）用cc189的memorization复杂度是O(n^2)。递归深度是S:O(n)。这种解法能够prune掉那些”aaaab“，dict = \["a", "aa", "aaa"]，这个canBreak表示从i到end一段可以break。不过对于那些”aaaaa“，dict = \["a", "aa", "aaa"]，还是得O(2^n)

```java
public List<String> wordBreak(String s, Set<String> wordDict) {
    List<String> res = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return res;
    }

    boolean[] canBreak = new boolean[s.length() + 1];
    Arrays.fill(canBreak, true);

    LinkedList<String> tmp = new LinkedList<>();
    searchRest(res, tmp, wordDict, canBreak, 0, s);

    return res;
}

private void searchRest(List<String> res, LinkedList<String> tmp, Set<String> wordDict, boolean[] canBreak, int start, String s) {
    if (start == s.length()) {
        StringBuilder sb = new StringBuilder();
        for (String ss : tmp) {
            sb.append(ss);
            sb.append(" ");
        }
        sb.deleteCharAt(sb.length() - 1);
        res.add(sb.toString());
        return;
    }

    if (!canBreak[start]) {
        return;
    }

    for (int i = start; i < s.length(); i++) {
        String cur = s.substring(start, i + 1);
        if (!wordDict.contains(cur) || !canBreak[i + 1]) {
            continue;
        }
        tmp.add(cur);
        int beforeLen = res.size();
        searchRest(res, tmp, wordDict, canBreak, start + cur.length(), s);
        if (beforeLen == res.size()) {
            canBreak[start + cur.length()] = false;
        }
        tmp.removeLast();
    }
}
```

另外在tutorial里看到了种比较好理解的解法，还有怎么优化：T:O(n^n), S:O(n ^ 3) --> T: O(n ^ 3), S: O(n ^ 3)

最坏情况是：aaa，然后字典里有\[a, aa]，那么每一个字符都要切开然后递归。递归数深度为n，然后每一层都有n个string，每个string长度为n。

```java
public List<String> wordBreak(String s, Set<String> wordDict) {
    return word_Break(s, wordDict, 0);
}
public List<String> word_Break(String s, Set<String> wordDict, int start) {
    LinkedList<String> res = new LinkedList<>();
    if (start == s.length()) {
        res.add("");
    }
    for (int end = start + 1; end <= s.length(); end++) {
        if (wordDict.contains(s.substring(start, end))) {
            List<String> list = word_Break(s, wordDict, end);
            for (String l : list) {
                res.add(s.substring(start, end) + (l.equals("") ? "" : " ") + l);
            }
        }
    }
    return res;
}
```

```java
public List<String> wordBreak(String s, Set<String> wordDict) {
    return word_Break(s, wordDict, 0);
}
HashMap<Integer, List<String>> map = new HashMap<>();

public List<String> word_Break(String s, Set<String> wordDict, int start) {
    if (map.containsKey(start)) {
        return map.get(start);
    }
    LinkedList<String> res = new LinkedList<>();
    if (start == s.length()) {
        res.add("");
    }
    for (int end = start + 1; end <= s.length(); end++) {
        if (wordDict.contains(s.substring(start, end))) {
            List<String> list = word_Break(s, wordDict, end);
            for (String l : list) {
                res.add(s.substring(start, end) + (l.equals("") ? "" : " ") + l);
            }
        }
    }
    map.put(start, res);
    return res;
}
```

下面是例子，如果用了hashmap优化，我们知道位置2已经搜过了，以后再搜的时候，直接返回

```
substring : a                   substring : a
substring : a                   substring : a
substring : a                   substring : a
substring : aa                  substring : aa
search this before : 3          
substring : aa                  substring : aa
search this before : 2          
substring : aaa                 substring : a
search this before : 3          
[a a a, a aa, aa a, aaa]        substring : aaa
                                [a a a, a aa, aa a, aaa]
```
