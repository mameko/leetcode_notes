# 454 4Sum II

Given four lists A, B, C, D of integer values, compute how many tuples`(i, j, k, l)`there are such that`A[i] + B[j] + C[k] + D[l]`is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228to 228- 1 and the result is guaranteed to be at most 231- 1.

**Example:**

```
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

本来以为这题跟Quad Combination一样，但其实不一样，因为位置不会重复（4个不同的array），所以不用判断，找到的全加上。

```java
// 几年后，又抄了一次，java8的
public int fourSumCount(int[] a, int[] b, int[] c, int[] d) {
    if (a == null || b == null || c == null || d == null) {
        return 0;
    }

    // <diff, count>
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < a.length; i++) {
        for (int j = 0; j < b.length; j++) {
            int sum = a[i] + b[j];
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
    }

    int totalCnt = 0;
    for (int i = 0; i < c.length; i++) {
        for (int j = 0; j < d.length; j++) {
            int sum = c[i] + d[j];
            totalCnt += map.getOrDefault(-sum, 0);
        }
    }

    return totalCnt;
}


public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
    if (A == null || B == null || C == null || D == null || A.length == 0 || B.length == 0 || C.length == 0 || D.length == 0) {
        return 0;
    }

    // <ABsum, num of results>
    HashMap<Integer, Integer> ABSums = new HashMap<>();
    int n = A.length;

    // store all possible sums A array & B array can produce
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int sum = A[i] + B[j];
            if (ABSums.containsKey(sum)) {
                ABSums.put(sum, ABSums.get(sum) + 1);
            } else {
                ABSums.put(sum, 1);
            }
        }
    }

    int res = 0;
    // compute sums C & D can produce, check whether -CDsum is in ABSums table.
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int CDSum = C[i] + D[j];
            CDSum = CDSum * -1;
            if (ABSums.containsKey(CDSum)) {
                res += ABSums.get(CDSum);
            }
        }
    }

    return res;
}
```
