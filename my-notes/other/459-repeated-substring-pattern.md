# 459 Repeated Substring Pattern

Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

**Example 1:**

```
Input: "abab"
Output: True

Explanation: It's the substring "ab" twice.
```

**Example 2:**

```
Input: "aba"
Output: False
```

**Example 3:**

```
Input: "abcabcabcabc"
Output: True

Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)
```

感觉这题是在暴力地枚举substring，搜到为止。用能否除尽substring长度来剪枝。能除尽的话，就另开一个loop看是否真的是由这个substring拼成的。

```java
public boolean repeatedSubstringPattern(String s) {
    if (s == null || s.isEmpty()) {
        return false;
    }

    int n = s.length();
    for (int len = 1; len <= n / 2; len++) {
        if (n % len != 0) {
            continue;
        }

        String pre = s.substring(0, len);
        int i = len;
        for (i = len; i <= n - len; i+=len) {
            String cur = s.substring(i, i + len);
            if (!cur.equals(pre)) {
                break;
            }
        }

        if (i == n) {
            return true;
        }
    }

    return false;
}
```
