# 775 Global and Local Inversions

We have some permutation `A` of `[0, 1, ..., N - 1]`, where `N` is the length of `A`.

The number of (global) inversions is the number of `i < j` with `0 <= i < j < N` and `A[i] > A[j]`.

The number of local inversions is the number of `i` with `0 <= i < N` and `A[i] > A[i+1]`.

Return `true` if and only if the number of global inversions is equal to the number of local inversions.

**Example 1:**

```
Input: A = [1,0,2]
Output: true
Explanation: There is 1 global inversion, and 1 local inversion.
```

**Example 2:**

```
Input: A = [1,2,0]
Output: false
Explanation: There are 2 global inversions, and 1 local inversion.
```

**Note:**

* `A` will be a permutation of `[0, 1, ..., A.length - 1]`.
* `A` will have length in range `[1, 5000]`.
* The time limit for this problem has been reduced.

这题一看，还以为是segment tree呢，然后，瞧了一眼提示，再画了几个栗子，发现，global 增长的比local快好多。所以只有swap相邻两个数的时候才有可能是相等。然后因为数字是从0到N - 1，这跟下标相对应。所以，只要判断数字跟下标的差就能知道是不是swap了相距超过1的数字。T:O(n), S:O(1)

```java
public boolean isIdealPermutation(int[] A) {
    if (A == null || A.length == 0) {
        return true;
    }

    for (int i = 0; i < A.length; i++) {
        if (Math.abs(A[i] - i) > 1) {
            return false;
        }
    }

    return true;
}
```
