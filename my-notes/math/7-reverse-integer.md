# 7 reverse Integer

Reverse digits of an integer. Returns 0 when the reversed integer overflows (signed 32-bit integer).

**Example**

Given`x = 123`, return`321`

Given`x = -123`, return`-321`

```java
public int reverseInteger(int n) {
    if (n == 0) {
        return n;
    }

    int res = 0;
    while (n != 0) {
        int temp = res * 10 + n % 10;
        n = n / 10;
        if (temp / 10 != res) {
            return 0;
        }
        res = temp;
    }

    return res;
}
```
