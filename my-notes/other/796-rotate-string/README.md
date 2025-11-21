# 796 Rotate String

We are given two strings,`A`and`B`.

A\_shift on`A`\_consists of taking string`A`and moving the leftmost character to the rightmost position. For example, if`A = 'abcde'`, then it will be`'bcdea'`after one shift on`A`. Return`True`if and only if`A`can become`B`after some number of shifts on`A`.

```
Example 1:
Input: A = 'abcde', B = 'cdeab'
Output: true

Example 2:
Input: A = 'abcde', B = 'abced'
Output: false
```

**Note:**

* `A`and`B`will have length at most`100`.

这题在cc189见过，如果是合法的rotattion的话，那个把原来的string连起来，那个rotate了的string会是那个的substring。例如：abcdeabcde里会有cdeab，如果不是合法的就不会是substring

```java
public boolean rotateString(String A, String B) {
    if (A == null || B == null) {
        return true;
    } else if (A.length() != B.length()) {
        return false;
    }

    String total = A + A;
    return total.indexOf(B) != -1;
}
```
