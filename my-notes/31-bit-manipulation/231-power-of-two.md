# 231 Power of Two

Given an integer, write a function to determine if it is a power of two.

记公式，参考[9 bit manipulation](../../templates/9-bit-manipulation.md)

```java
public boolean isPowerOfTwo(int n) {
    if (n <= 0) {
        return false;
    }

    return (n & (n - 1)) == 0;
}
```
