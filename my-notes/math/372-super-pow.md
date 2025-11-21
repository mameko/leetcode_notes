# 372 Super Pow

Your task is to calculate a^b mod 1337 where a is a positive integer and b is an extremely large positive integer given in the form of an array.

**Example1:**

```
a = 2
b = [3]

Result: 8
```

**Example2:**

```
a = 2
b = [1,0]

Result: 1024
```

利用mod的性质，a^123 % k = (a ^ 120) % k × (a ^ 3) % k = (((a ^ 12) % k) ^ 10 % k) × (a ^ 3) % k。这题可以递归着做。不看答案真没想法。

```java
int base = 1337;
public int superPow(int a, int[] b) {
    if (b == null || b.length == 0) {
        return 1;
    }

    return superPowHelper(a, b, b.length - 1);// 注意下标
}

private int superPowHelper(int a, int[] b, int i) {
    if (i < 0) {// 注意下标
        return 1;
    }
    return modPow(superPowHelper(a, b, i - 1), 10) % base * modPow(a, Integer.valueOf(b[i])) % base;
}

private int modPow(int a, int k) {
    a = a % base;
    int result = 1;
    while (k > 0) {
        result = (result * a) % base;
        k--;
    }

    return result;
}
```
