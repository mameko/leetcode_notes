# 242 Valid Anagram

Given two stringssandt, write a function to determine iftis an anagram ofs.

For example,\
s= "anagram",t= "nagaram", return true.\
s= "rat",t= "car", return false.

**Note:**\
You may assume the string contains only lowercase alphabets.

**Follow up:**\
What if the inputs contain unicode characters? How would you adapt your solution to such case?

这题用了一个hashmap数source里每个char出现次数，然后loop一遍target，看到一个减一个，如果target出现了source里没有的字符，返回false。如果出现的字符已经减为0，表示target里同样的字符出现的次数比soruce里的次数多，返回false。T:O(n)，S:O(n)因为只含有26个小写字母，所以可以看成constant space。下面再提供一种建数组的写法。

```java
public boolean isAnagram(String s, String t) {
    if (s == t) {
        return true;
    }

    if (s == null && t != null) {
        return false;
    } else if (s != null && t == null) {
        return false;
    } else if (s.equals(t)) {
        return true;
    } else if (s.length() != t.length()) {
        return false;
    }

    HashMap<Character, Integer> sourceMap = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        char cur = s.charAt(i);
        if (sourceMap.containsKey(cur)) {
            sourceMap.put(cur, sourceMap.get(cur) + 1);
        } else {
            sourceMap.put(cur, 1);
        }
    }

    for (int j = 0; j < t.length(); j++) {
        char cur = t.charAt(j);
        if (sourceMap.containsKey(cur)) {
            if (sourceMap.get(cur) > 0) {
                sourceMap.put(cur, sourceMap.get(cur) - 1);
            } else {
                return false;
            }
        } else {
            return false;
        }
    }

    return true;

}
```

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] counter = new int[26];
    for (int i = 0; i < s.length(); i++) {
        counter[s.charAt(i) - 'a']++;
        counter[t.charAt(i) - 'a']--;
    }
    for (int count : counter) {
        if (count != 0) {
            return false;
        }
    }
    return true;
}
```

```java
public boolean isAnagram(String s, String t) {
    if (s == null && t == null) {
        return true;
    } else if (s == null || t == null) {
        return false;
    } else if (s.length() != t.length()) {
        return false;
    }

    int[] hm = new int[26];
    for (int i = 0; i < s.length(); i++) {
        hm[s.charAt(i) - 'a']++;
    }

    for (int i = 0; i < t.length(); i++) {
        hm[t.charAt(i) - 'a']--;
        if (hm[t.charAt(i) - 'a'] < 0) {
            return false;
        }
    }

    return true;
}
```
