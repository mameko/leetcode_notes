# 28 Implement strStr

Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

这题有3种做法，当然，我是写不出KMP的，所以只留了2种代码。

普通的O（n \* m）

```java
public int strStr(String haystack, String needle) {
    if (haystack == null || needle == null) {
        return -1;
    }

    if (needle.length() == 0) {
        return 0;
    }

    for (int i = 0; i < haystack.length() - needle.length() + 1; i++) {
        int j = 0;
        for (j = 0; j < needle.length(); j++) {
            if (haystack.charAt(i + j) != needle.charAt(j)) {
                break;
            }
        }

        if (j == needle.length()) {
            return i;
        }
    }

    return -1;
}

// 就换个loop的方式：
public int strStr(String haystack, String needle) {
    if (haystack == null || needle == null) {
        return -1;
    }

    if (needle.length() == 0) {
        return 0;
    }

    for (int i = 0; i < haystack.length() - needle.length() + 1; i++) {
        int j = 0;
        while (j < needle.length() && haystack.charAt(i + j) == needle.charAt(j)) {
            j++;
        }

        if (j == needle.length()) {
            return i;
        }
    }

    return -1;
}
```

Rabin-Karp: worse case O（n \* m）,avg O(m + n)：利用了hashcode的性质。

```java
public int strStr2(String source, String target) {
    if (source == null || target == null) {
        return -1;
    }

    int m = target.length();
    int n = source.length();
    if (m == 0) {
        return 0;
    }

    int BASE = 100000;

    // calculate 31 ^ m
    int power = 1;
    for (int i = 0; i < m; i++) {
        power = (power * 31) % BASE;
    }

    // calculate target hash code
    int targetCode = 0;
    for (int i = 0; i < m; i++) {
        targetCode = ((targetCode * 31) + target.charAt(i)) % BASE;
    }

    // calculate source hash code
    int curCode = 0;
    for (int i = 0; i < n; i++) {
        curCode = ((curCode * 31) + source.charAt(i)) % BASE;

        // not have enough chars
        if (i < m) {
            continue;
        }

        // we have enough chars, from now on, we need to calculate new hash code each step
        if (i >= m) {
            curCode = curCode - (source.charAt(i - m) * power) % BASE;
            if (curCode < 0) {
                curCode += BASE;
            }
        }

        // now if we have hash code match, we need to check whether we have same string
        if (targetCode == curCode) {
            if (target.equals(source.substring(i - m + 1, i + 1))) {
                return i - m + 1;
            }
        }
    }

    return -1;
}
```
