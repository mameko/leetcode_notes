# 205 Isomorphic Strings

Given two strings**s**and**t**, determine if they are isomorphic.

Two strings are isomorphic if the characters in**s**can be replaced to get**t**.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

For example,\
Given`"egg"`,`"add"`, return true.

Given`"foo"`,`"bar"`, return false.

Given`"paper"`,`"title"`, return true.

**Note:**\
You may assume both **s** and **t** have the same length.

这里用了两个hashmap，首先判断如果两个string相等，那么它们就一定是isomorphic strings。然后如果它们的长度不相等就一定不是。然后我们把两条string loop一遍。一边loop一边把src跟target中字母的对应关系存到hashmap里。如果发现forward hm里存在同一个src的字母对应不同target字母的情况的话，返回false。每当遇到新的src字母时，要检查一下该target字母是否已经被map了，如果是的话返回false。最后把遇到没map的src和target字母存两个方向。

```java
// 2刷：
public boolean isIsomorphic(String s, String t) {
    if (s == null && t == null) {
        return true;
    } else if (s == null || t == null) {
        return false;
    } else if (s.length() != t.length()) {
        return false;
    }

    Map<Character, Character> forward = new HashMap<>();
    Map<Character, Character> backward = new HashMap<>();

    for (int i = 0; i < s.length(); i++) {
        char from = s.charAt(i);
        char to = t.charAt(i);
        if (forward.containsKey(from)) {
            if (forward.get(from) != to) {
                return false;
            }
        } else {
            if (backward.containsKey(to)) {
                return false;
            } else {
                forward.put(from, to);
                backward.put(to, from);
            }
        }
    }

    return true;
}

// 1刷：
public boolean isIsomorphic(String s, String t) {
    if (s == null && t == null) {
        return true;
    } else if (s == null || t == null) {
        return false;
    }

    if (s.equals(t)) {
        return true;
    }

    if (s.length() != t.length()) {
        return false;
    }

    HashMap<Character, Character> hmForward = new HashMap<>();
    HashMap<Character, Character> hmBackword= new HashMap<>();
    char[] ss = s.toCharArray();
    char[] ts = t.toCharArray();

    for (int i = 0; i < ss.length; i++) {
        if (hmForward.containsKey(ss[i])) {
            if (hmForward.get(ss[i]) != ts[i]) {
                return false;
            }
        } else {
            if (hmBackword.get(ts[i]) != ss[i]) {
                return false;
            }
            hmForward.put(ss[i], ts[i]);
            hmBackword.put(ts[i], ss[i]);
        }
    }

    return true;
}
```
