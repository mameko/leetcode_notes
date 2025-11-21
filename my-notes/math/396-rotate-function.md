# 396 Rotate Function

Given an array of integers`A`and letnto be its length.

Assume`Bk`to be an array obtained by rotating the array`A`kpositions clock-wise, we define a "rotation function"`F`on`A`as follow:

`F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]`.

Calculate the maximum value of`F(0), F(1), ..., F(n-1)`.

**Note:**\
nis guaranteed to be less than 105.

**Example:**

```
A = [4, 3, 2, 6]

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26

So the maximum value of F(0), F(1), F(2), F(3) is F(3) = 26.
```

这题直接做O(n^2)会TEL。这是超时的代码。

```java
public int maxRotateFunction(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }

    int n = A.length;
    int max = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        int curSum = 0;
        for (int j = 0; j < n; j++) {
            curSum += ((j + i) % n) * A[j];
        }

        max = Math.max(curSum, max);
    }

    return max;
}
```

看了discuss的答案，原来是得推导的，推了发现每个sum之间的关系，然后可以把复杂度降低到O(n)：

首先把式子列出来。

```
F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]
F(k-1) = 0 * Bk-1[0] + 1 * Bk-1[1] + ... + (n-1) * Bk-1[n-1]
       = 0 * Bk[1] + 1 * Bk[2] + ... + (n-2) * Bk[n-1] + (n-1) * Bk[0]
```

然后，相减发现规律：

```
F(k) - F(k-1) = Bk[1] + Bk[2] + ... + Bk[n-1] + (1-n)Bk[0]
              = (Bk[0] + ... + Bk[n-1]) - nBk[0]
              = sum - nBk[0]
```

移项可得：

```
F(k) = F(k-1) + sum - nBk[0]
```

然后找Bk\[0]：

```
k = 0; B[0] = A[0];
k = 1; B[0] = A[len-1];
k = 2; B[0] = A[len-2];
...
```

最后代码是：

```java
int allSum = 0;
int len = A.length;
int F = 0;
for (int i = 0; i < len; i++) {
    F += i * A[i];
    allSum += A[i];
}
int max = F;
for (int i = len - 1; i >= 1; i--) {
    F = F + allSum - len * A[i];
    max = Math.max(F, max);
}
return max;
```
