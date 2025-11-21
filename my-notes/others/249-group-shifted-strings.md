# 249 Group Shifted Strings

Given a string, we can "shift" each of its letter to its successive letter, for example:`"abc" -> "bcd"`. We can keep "shifting" which forms the sequence:

```
"abc" -> "bcd" -> ... -> "xyz"
```

Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

For example, given:`["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"]`,\
A solution is:

```
[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
```

这题要按照字母之间位移量来把字符串分类。这里主要得注意，ba和az的情况，因为b -> a的位移是-25，我们要加上26，mod一下，把它跟az归到一组。T:O(n\* avg len of string), S:O(n)

```java
public List<List<String>> groupStrings(String[] strings) {
    List<List<String>> res = new ArrayList<>();
    if (strings == null || strings.length == 0) {
        return res;
    }

    // group keys according to shifted amount between chars
    HashMap<String, List<String>> hm = new HashMap<>();
    for (String item : strings) {
        String key = "";
        for (int i = 1; i < item.length(); i++) {
            int diff = item.charAt(i) - item.charAt(i - 1);
            if (diff < 0) {
                diff = diff + 26;
            }
            key = key + "," + String.valueOf(diff);
        }

        if (hm.containsKey(key)) {
            hm.get(key).add(item);
        } else {
            List<String> tmp = new ArrayList<>();
            tmp.add(item);
            hm.put(key, tmp);
        }
    }

    // put them in res
    for (Map.Entry<String, List<String>> entry : hm.entrySet()) {
        res.add(entry.getValue());
    }

    return res;
}
```
