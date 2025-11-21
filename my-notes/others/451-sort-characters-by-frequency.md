# 451 Sort Characters By Frequency

Given a string, sort it in decreasing order based on the frequency of characters.

**Example 1:**

```
Input:"tree"
Output:"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```

**Example 2:**

```
Input:"cccaaa"
Output:"cccaaa"

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.
```

**Example 3:**

```
Input:"Aabb"
Output:"bbAa"

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.
```

这题就用hashmap数一次频率，然后按freq排队，然后返回结果，感觉处理起来有点复杂。

```java
public String frequencySort(String s) {
    if (s == null || s.isEmpty()) {
        return "";
    }

    int n = s.length();
    // collect freq
    HashMap<Character, Integer> freqMap = new HashMap<>();
    for (int i = 0; i < n; i++) {
        char c = s.charAt(i);
        if (freqMap.containsKey(c)) {
            freqMap.put(c, freqMap.get(c) + 1);
        } else {
            freqMap.put(c, 1);
        }
    }

    // reverse the map
    HashMap<Integer, StringBuilder> freqToChar = new HashMap<>();
    for (Map.Entry<Character, Integer> e : freqMap.entrySet()) {
        int freq = e.getValue();
        char ch = e.getKey();

        StringBuilder sb;
        if (freqToChar.containsKey(freq)) {
             sb = freqToChar.get(freq);
        } else {
             sb = new StringBuilder();
        }

        for (int i = 0; i < freq; i++) {
            sb.append(ch);
        }

        freqToChar.put(freq, sb);
    }

    // sort frequency in descending order
    List<Integer> keySet = new ArrayList<>(freqToChar.keySet());
    Collections.sort(keySet, Collections.reverseOrder());

    // collect result
    String res = "";
    for (Integer key : keySet) {
        res = res + freqToChar.get(key).toString();
    }

    return res;
}
```
