# 29 Divide Two Integers

Divide two integers without using multiplication, division and mod operator.

If it is overflow, return`2147483647`

**Example**

Given dividend =`100`and divisor =`9`, return`11`.

```java
public int divide(int dividend, int divisor) {
    if (divisor == 0) {
        return dividend >= 0 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
    }

    if (dividend == Integer.MIN_VALUE && divisor == -1) {
        return Integer.MAX_VALUE;
    }

    if (dividend == 0) {
        return 0;
    }

    long a = Math.abs((long)dividend);
    long b = Math.abs((long)divisor);
    boolean isNegative = (dividend < 0 && divisor > 0) || (dividend > 0 && divisor < 0);

    int res = 0;
    while (a >= b) {
        int shift = 0;
        while (a >= (b << shift)) {
            shift++;
        }
        res += 1 << (shift - 1);
        a -= b << (shift - 1);
    }

    return isNegative ? -res : res;
}
```
