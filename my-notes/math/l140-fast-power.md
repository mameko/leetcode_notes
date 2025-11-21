# L140 Fast Power

Calculate the **a^n % b** where a, b and n are all 32bit integers.

**Example**

For 2^31% 3 = 2

For 100^1000% 1000 = 0

[**Challenge**](http://www.lintcode.com/en/problem/fast-power/#challenge)

O(logn)

这题，很迷茫，用的是mod的性质，乘起来再mod跟分开mod再乘是一样的。然后遵循那个logn求平方的思想（[L428](l428-pow-x-n.md)），循环地来了一遍求平方。还贴了九章的递归班答案，感觉还是递归比较容易理解。

过了几年又去上九章，再认真看了一遍。其实loop的版本，是把幂当成2进制的数字来看。譬如，3^10，10的二进制是1010，那么3^10 = 3^8 \* 3^2 = (3的2^3 **\* 1**) \* (3的2^2 \* 0) \* (3的2^1 **\* 1**) \* (3的2^0 \* 0) 。所以每次loop我们把这个n右移一位，1010=》101=》10=》1，因为n右移一位，等于base \* base double了一下。然后遇到了基数幂单独拿出来乘到result里，就是上面&#x7684;**\*1**的那些。

至于logn的递归，是每次如果是奇数，我们乘以一个a，偶数，我们乘以两个a

```java

public int fastPower(int a, int b, int n) {
    if (n == 1) {
        return a % b;
    }
    if (n == 0) {
        return 1 % b;
    }

    long base = a;
    long res = 1;

    while (n > 0) {
        if (n % 2 == 1) {
            res = (res * base) % b;
        }

        base = (base * base) % b;
        n = n / 2;
    }

    return (int)res;
}


public int fastPower(int a, int b, int n) {
    if (n == 1) {
        return a % b;
    }
    if (n == 0) {
        return 1 % b;
    }

    long product = fastPower(a, b, n / 2);
    product = (product * product) % b;
    if (n % 2 == 1) {
        product = (product * a) % b;
    }
    return (int) product;
}
```

```java
public int fastPower(int a, int b, int n) {
    if (b == 0) {// 不能除0，所以返回MIN
        return Integer.MIN_VALUE;
    }

    if (b == 1) {// 所有数模1都等于0
        return 0;
    }

    if (n == 0) {// 所有数的0次都等于1
        return 1 % b;
    }

    if (n == 1) {// 如果是一次方，那么直接返回mod的结果
        return a % b;
    }

    long result = 1;
    long base = a % b;
    while (n > 0) {
        if (n % 2 != 0) {
            result = (result * base) % b;
        }
        n = n >>> 1;
        base = (base * base) % b;
    }
    return (int) result;
}
```
