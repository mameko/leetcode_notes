# 49 Group Anagrams

Given an array of strings, group anagrams together.

For example, given:`["eat", "tea", "tan", "ate", "nat", "bat"]`,\
Return:

```
[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:**&#x41;ll inputs will be in lower-case.

把每个字符串排序，作为key，然后把所有排序后相同的放在一起，最后loop一遍把map里的倒出来返回结果。T:O(n \* avglen log avg len), S:O(n)

```java
public List<List<String>> groupAnagrams(String[] strs) {
    List<List<String>> res = new ArrayList<>();
    if (strs == null || strs.length == 0) {
        return res;
    }
    // <sorted string, list of String>
    HashMap<String, List<String>> hm = new HashMap<>();
    for (String s : strs) {
        char[] cs = s.toCharArray();
        Arrays.sort(cs);
        String key = new String(cs);
        List<String> tmp;
        if (hm.containsKey(key)) {
            tmp = hm.get(key);
        } else {
            tmp = new ArrayList<>();
        }

        tmp.add(s);
        hm.put(key, tmp);
    }

    for (Map.Entry<String, List<String>> entry : hm.entrySet()) {
        res.add(entry.getValue());
    }

    return res;
}
```
