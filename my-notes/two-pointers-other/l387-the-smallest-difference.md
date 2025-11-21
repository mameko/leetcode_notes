# L387 The Smallest Difference

Given two array of integers(the first array is array`A`, the second array is array`B`), now we are going to find a element in array A which is A\[i], and another element in array B which is B\[j], so that the difference between A\[i] and B\[j] (|A\[i] - B\[j]|) is as small as possible, return their smallest difference.

**Example**

For example, given array A =`[3,6,7,4]`, B =`[2,8,9,3]`, return`0`

[**Challenge**](http://www.lintcode.com/en/problem/the-smallest-difference/#challenge)

O(_n\_log\_n_) time

先排序，然后再两个指针来扫两个数组，边扫边找。

```java
public int smallestDifference(int[] A, int[] B) {
    if (A == null || B == null || A.length == 0 || B.length == 0) {
        return 0;
    } 

    Arrays.sort(A);
    Arrays.sort(B);

    int i = 0;
    int j = 0;
    int min = Integer.MAX_VALUE;
    while (i < A.length && j < B.length) {
        int diff = Math.abs(A[i] - B[j]);
        min = Math.min(min, diff);
        //谁小谁往前移动，因为移动大的那个会把diff差距拉大而已，移动小的那个才有可能缩小差距。
        if (A[i] < B[j]) {
            i++;
        } else {
            j++;
        }
    }

    return min;
}
```
