# 290 Word Pattern

Given a`pattern`and a string`str`, find if`str`follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in`pattern`and a **non-empty** word in`str`.

**Examples:**

1. pattern =`"abba"`, str =`"dog cat cat dog"`should return true.
2. pattern =`"abba"`, str =`"dog cat cat fish"`should return false.
3. pattern =`"aaaa"`, str =`"dog cat cat dog"`should return false.
4. pattern =`"abba"`, str =`"dog dog dog dog"`should return false.

**Notes:**\
You may assume`pattern`contains only lowercase letters, and`str`contains lowercase letters separated by a single space.

这里就用一个hashmap把字母与单词的对应关系存起来。边loop边检查已经对应好的关系是否被破坏。例如已经被map好的字母对应不同的单词；又或者字母还没出现但对应的单词已经出现。T：O(m), S:O(m) m是pattern的长度。

```java
public boolean wordPattern(String pattern, String str) {
    if (str == null && pattern == null) {
        return true;
    }

    String[] strings = str.split(" ");
    char[] cs = pattern.toCharArray();

    if (strings.length != cs.length) {
        return false;
    }

    // <char, word>
    HashMap<Character, String> hm = new HashMap<>();
    for (int i = 0; i < cs.length; i++) {
        Character curC = cs[i];
        String curS = strings[i];

        if (hm.containsKey(curC)) {
            if (!hm.get(curC).equals(curS)) {
                return false;
            }
        } else {
            if (hm.containsValue(curS)) {
                return false;
            }
        }
        hm.put(curC, curS);
    }

    return true;
}
```
