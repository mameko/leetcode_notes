# 291 Word Pttern II

Given a`pattern`and a string`str`, find if`str`follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in`pattern`and a **non-empty** substring in`str`.

**Examples:**

1. pattern =`"abab"`, str =`"redblueredblue"`should return true.
2. pattern =`"aaaa"`, str =`"asdasdasdasd"`should return true.
3. pattern =`"aabb"`, str =`"xyzabcxzyabc"`should return false.

**Notes:**\
You may assume both`pattern`and`str`contains only lowercase letters.

这题跟290的判断条件一样，但因为不能把输入字符串按空格分开，所以要一边递归一边backtrack。时间复杂度好像是O(n^n)

```java
public boolean wordPatternMatch(String pattern, String str) {
    Map<Character, String> mapping = new HashMap<>();
    return helper(pattern, 0, str, 0, mapping);
}

private boolean helper(String pattern, int pi, String str, int si, Map<Character, String> mapping) {
    if (pi == pattern.length() && si == str.length()) {
        return true;
    } else if (pi == pattern.length() || si == str.length()) {
        return false;
    }

    char pcur = pattern.charAt(pi);
    for (int i = si + 1; i <= str.length(); i++) {
        String scur = str.substring(si, i);
        if (mapping.containsKey(pcur)) {
            if (mapping.get(pcur).equals(scur)) {
                if (helper(pattern, pi + 1, str, i, mapping)) {
                    return true;
                }
            }
        } else {
            if (mapping.containsValue(scur)) {
                continue;
            }
            mapping.put(pcur, scur);
            if (helper(pattern, pi + 1, str, i, mapping)) {
                return true;
            }
            mapping.remove(pcur);
        }

    }

    return false;
}
```
