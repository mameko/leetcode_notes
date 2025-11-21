# L10 String Permutation II

Given a string, find all permutations of it without duplicates.

**Example**

Given`"abb"`, return`["abb", "bab", "bba"]`.

Given`"aabb"`, return`["aabb", "abab", "baba", "bbaa", "abba", "baab"]`.

跟permutation II 很相似，只是把数字换成了字母而已。

```java
public List<String> stringPermutation2(String str) {
    List<String> res = new ArrayList<>();
    if (str == null || str.length() == 0) {
        res.add("");
        return res;
    }

    char[] cs = str.toCharArray();
    Arrays.sort(cs);
    boolean[] visited = new boolean[str.length()];
    StringBuilder tmp = new StringBuilder();
    dfsHelper(tmp, res, visited, cs, 0);

    return res;
}

private void dfsHelper(StringBuilder tmp, List<String> res, boolean[] visited, 
                       char[] cs, int start) {
    if (tmp.length() == cs.length) {
        res.add(tmp.toString());
        return;
    }

    for (int i = 0; i < cs.length; i++) {
        // 去重
        if (i != 0 && !visited[i - 1] && cs[i] == cs[i - 1]) {
            continue;
        }

        if (!visited[i]) {
            tmp.append(cs[i]);
            visited[i] = true;
            dfsHelper(tmp, res, visited, cs, start + 1);//递归选下个
            visited[i] = false;
            tmp.deleteCharAt(tmp.length() - 1);
        }
    }
}
```
