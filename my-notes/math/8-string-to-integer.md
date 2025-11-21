# 8 String to Integer

Implement atoi to convert a string to an integer.

**Hint:**&#x43;arefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

**Notes:**&#x49;t is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.

首先得去掉leading space，然后把符号记录好。然后才开始算具体数字。这里算数字的方法跟calculater的差不多，如果看到不是数字的字符，我们不继续下去，把之前process好的部分返回就好了。如果我们遇到超出范围的数字时，我们就返回最大/最小值。感觉这个判断方法跟[reverse Integer](7-reverse-integer.md)的有点相似。T:O(n),S:O(1)

```java
public int myAtoi(String str) {
    if (str == null || str.isEmpty()) {
        return 0;
    }

    int n = s.length();
    int cur = 0;
    while (cur < n && s.charAt(cur) == ' ') { // skip white space in front
        cur++;
    }        
    
    int sign = 1;
    if (cur < n && s.charAt(cur) == '-') {
        cur++;
        sign = -1;
    } else if (cur < n && s.charAt(cur) == '+') {
        cur++;
    }

    int res = 0;
    int base = 10;
    for (int i = cur; i < n; i++) {
        char c = str.charAt(i);
        if (!Character.isDigit(c)) {
            return res * sign;
        }

        int dig = Character.getNumericValue(c);
        // skip value that is too big or too small
        if (res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE / 10 && dig > 7)) {
            return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
        }
        res = base * res + dig;
    }

    return res * sign;
}
```
