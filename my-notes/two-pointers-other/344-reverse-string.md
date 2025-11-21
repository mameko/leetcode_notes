# 344 Reverse String

Write a function that takes a string as input and returns the string reversed.

**Example:**\
Given s = "hello", return "olleh".

没什么特别，reverse模板直接上。

```java
public String reverseString(String s) {
    if (s == null || s.length() == 0) {
        return s;
    }

    char[] cs = s.toCharArray();
    for (int i = 0, j = cs.length - 1; i < j; i++, j--) {
        char tmp = cs[i];
        cs[i] = cs[j];
        cs[j] = tmp;
    }

    return new String(cs);
}
```
