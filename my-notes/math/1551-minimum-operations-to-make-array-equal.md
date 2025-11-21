# 1551 Minimum Operations to Make Array Equal

You have an array `arr` of length `n` where `arr[i] = (2 * i) + 1` for all valid values of `i` (i.e. `0 <= i < n`).

In one operation, you can select two indices `x` and `y` where `0 <= x, y < n` and subtract `1` from `arr[x]` and add `1` to `arr[y]` (i.e. perform `arr[x] -=1` and `arr[y] += 1`). The goal is to make all the elements of the array **equal**. It is **guaranteed** that all the elements of the array can be made equal using some operations.

Given an integer `n`, the length of the array. Return _the minimum number of operations_ needed to make all the elements of arr equal.

**Example 1:**

```
Input: n = 3
Output: 2
Explanation: arr = [1, 3, 5]
First operation choose x = 2 and y = 0, this leads arr to be [2, 3, 4]
In the second operation choose x = 2 and y = 0 again, thus arr = [3, 3, 3].
```

**Example 2:**

```
Input: n = 6
Output: 9
```

**Constraints:**

* `1 <= n <= 10^4`

这题，只要把数字写出来分析一下就会发现规律。因为有数学解法，所以归类到这里。因为生成的数组是对称的。而且每次操作可以同时+1，-1。所以只要找到中位数，算前一半的数字要加多少次才能等于中位数就ok了。T:O(n), S:O(1)

```java
public int minOperations(int n) {
    if (n < 1) {
        return Integer.MAX_VALUE;
    }

    int count = 0;
    for (int i = 0; i < n / 2; i++) {
        int val = 2 * i + 1;
        count = count + n - val;
    }

    return count;
}

// 数学解：
public int minOperations(int n) {
    return n % 2 == 0 ? n * n / 4 : (n * n - 1) / 4;
}
```
