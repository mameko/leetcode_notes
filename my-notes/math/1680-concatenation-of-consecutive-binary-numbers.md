# 1680 Concatenation of Consecutive Binary Numbers

Given an integer `n`, return _the **decimal value** of the binary string formed by concatenating the binary representations of_ `1` _to_ `n` _in order, **modulo**_ `10^9 + 7`.

**Example 1:**

```
Input: n = 1
Output: 1
Explanation: "1" in binary corresponds to the decimal value 1. 
```

**Example 2:**

```
Input: n = 3
Output: 27
Explanation: In binary, 1, 2, and 3 corresponds to "1", "10", and "11".
After concatenating them, we have "11011", which corresponds to the decimal value 27.
```

**Example 3:**

```
Input: n = 12
Output: 505379714
Explanation: The concatenation results in "1101110010111011110001001101010111100".
The decimal value of that is 118505380540.
After modulo 109 + 7, the result is 505379714.
```

**Constraints:**

* `1 <= n <= 105`

这题一看，不就是数位转换吗？然后屁颠屁颠地写了。看了solution才发现还有个更高效的方法。

方法一：brute force，把数字一个一个转换成二进制，然后算总数。T:O(nlogn)，因为转二进制要logn，n个数，所以nlogn。S：O(n)。这里注意charAt返回的是char，要减‘0’

```java
public int concatenatedBinary(int n) {
    if (n < 1) {
        return 0;
    }

    int MOD = 1000000007;
    int total = 0;
    for (int i = 1; i <= n; i++) {
        String binaryStr = Integer.toBinaryString(i);
        int len = binaryStr.length();
        for (int j = 0; j < len; j++) {
            total = ((total * 2) % MOD + (binaryStr.charAt(j) - '0' % MOD)) % MOD;
        }
    }

    return total;
}
```

方法二：用bit manipulation。其实我们可以不真的把数字变成二进制，而是用shift和or来‘加'。首先通过观察，我们知道，二进制每次变长都是在2的n次方处。比如，1->1，2->2^1->10, 2的二进制比1长了1；3->11,4->2^2->100。4是2的平方，比3的二进制reprensentaion长了1。那么怎么快速找到这个数是2的次方呢？x & (x -1) == 0.然后我们怎么加呢？其实，shift以后，直接or就能加上

![](<../../.gitbook/assets/image (3).png>)

T:O(n) 因为我们算了n次。S:O(1)因为直接在result上做位运算，所以并没有产生string啥的。

```java
public int concatenatedBinary(int n) {
    final int MOD = 1000000007;
    int length = 0; // bit length of addends
    long result = 0; // long accumulator
    for (int i = 1; i <= n; i++) {
        // when meets power of 2, increase the bit length
        if ((i & (i - 1)) == 0) {
            length++;
        }
        result = ((result << length) | i) % MOD;
    }
    return (int) result;
}
```
