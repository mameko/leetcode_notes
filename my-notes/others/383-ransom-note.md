# 383 Ransom Note

Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

**Note:**\
You may assume that both strings contain only lowercase letters.

```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```

就是先把magazine的字母用数一遍，放到hm里，然后loop一次ransomNote，看字母够不够用。

```java
public boolean canConstruct(String ransomNote, String magazine) {
    if (ransomNote == null || magazine == null) {
        return false;
    }

    Map<Character, Integer> hm = new HashMap<>();
    // count char in magazine
    for (char c : magazine.toCharArray()) {
        if (hm.containsKey(c)) {
            hm.put(c, hm.get(c) + 1);
        } else {
            hm.put(c, 1);
        }
    }

    // check if we have enough chars for ransomNote
    for (char c : ransomNote.toCharArray()) {
        if (hm.containsKey(c)) {
            int cnt = hm.get(c) - 1;
            if (cnt < 0) {
                return false;
            }
            hm.put(c, cnt);
        } else {
            return false;
        }
    }

    return true;
}
```
