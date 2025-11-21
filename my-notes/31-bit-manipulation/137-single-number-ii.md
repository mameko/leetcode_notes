# 137 Single Number II

## 137 Single Number II

## Description

Given`3*n + 1`numbers, every numbers occurs triple times except one, find it.

## Example

Given`[1,1,2,3,3,3,2,2,4,1]`return`4`

## Challenge

One-pass, constant extra space.

这题利用的是移位的特性，还有这是一个int，所以总共32位。统计每一位出现的数目。如果%3不等于0的话，证明那一位是多出来那个数的一位。最后把那一位移到正确数位，就ok啦。

```java
 public int singleNumberII(int[] A) {
    if (A == null || A.length == 0) {
        return Integer.MAX_VALUE;
    }

    int[] bits = new int[32];
    int res = 0;
    for (int i = 0; i < 32; i++) {
        int  sum = 0;
        for (int j = 0; j < A.length; j++) {
            sum += (A[j] >> i) & 1;
        }

        res |= (sum % 3) << i;
    }

    return res;
}
```
