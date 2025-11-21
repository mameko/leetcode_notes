# 161 One Edit Distance

Given two strings S and T, determine if they are both one edit distance apart.

感觉这题边界控制有点难度。这个是得刚好等于1，一开始以为<=1。一直调不过。T:O(n)，S:O(1)

```java
public boolean isOneEditDistance(String s, String t) {
    if (s == null || t == null) {
        return false;
    } 

    int n = s.length();
    int m = t.length();

    if (Math.abs(n - m) > 1) {
        return false;
    }

    int i = 0;
    int j = 0;
    int diff = 0;

    while (i < n && j < m) {
        if (s.charAt(i) != t.charAt(j)) {
            diff++;

            // 其实这里只是快点return，不是必要的
            if (diff > 1) {
               return false;
            }　

            if (n > m) {
                i++;
            } else if (n < m) {
                j++;
            } else {
                i++;
                j++;
            }
        } else {
            i++;
            j++;
        }

    }

    while (i < n || j < m) {
        diff++;
        i++;
        j++;
    }

    return diff == 1;
}
```
