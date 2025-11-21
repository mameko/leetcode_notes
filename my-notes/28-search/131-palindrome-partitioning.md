# 131 Palindrome Partitioning

Given a strin&#x67;_&#x73;_, partition\_s\_such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

**Example**

Given s =`"aab"`, return:

```
[
  ["aa","b"],
  ["a","a","b"]
]
```

二刷时候刚做完前面那题[93 Restore IP Address](93-restore-ip-addresses.md) 然后发现，其实我们可以不传index的。因为每一层都把string砍一截，所以最后string的长度会变成0.

```java
public List<List<String>> partition(String s) {
    List<List<String>> res = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return res;
    }

    List<String> tmp = new ArrayList<>();
    dfs(res, tmp, s);
    return res;
}

private void dfs(List<List<String>> res, List<String> tmp, String s) {
    if (s.length() == 0) {
        res.add(new ArrayList<>(tmp));
        return;
    }

    for (int i = 1; i <= s.length(); i++) {
        String part = s.substring(0, i);
        if (!isPar(part)) {
            continue;
        }

        tmp.add(part);
        dfs(res, tmp, s.substring(i));
        tmp.remove(tmp.size() - 1);
    }
}

private boolean isPar(String s) {
    int len = s.length();
    for (int i = 0; i < len / 2; i++) {
        if (s.charAt(i) != s.charAt(len - i - 1)) {
            return false;
        }
    }

    return true;
}
```

就是简单暴力地搜。

```java
public List<List<String>> partition(String s) {
    List<List<String>> res = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return res;
    }

    List<String> tmp = new ArrayList<>();
    if (s.length() == 1) {
        tmp = new ArrayList<>();
        tmp.add(s);
        res.add(tmp);
        return res;
    }
    searchRest(s, tmp, res, 0);
    return res;
}

private boolean isPalindrom(String s) {
    for (int i = 0; i < s.length() / 2; i++) {
        if (s.charAt(i) != s.charAt(s.length() - i - 1)) {
            return false;
        }
    }
    return true;
}

private void searchRest(String s, List<String> tmp, List<List<String>> res, int start) {
    if (s.length() == start) {
        res.add(new ArrayList<>(tmp));
        return;
    }

    for (int i = start; i < s.length(); i++) {
        String cur = s.substring(start, i + 1);
        if (isPalindrom(cur)) {
            tmp.add(cur);
            searchRest(s, tmp, res, start + cur.length());
            tmp.remove(tmp.size() - 1);
        }
    }
}
```
