# L6 Merge Two Sorted Arrays

Merge two given sorted integer array\_A\_and\_B\_into a new sorted integer array.

**Example**

A=`[1,2,3,4]`

B=`[2,4,5,6]`

return`[1,2,2,3,4,4,5,6]`

[**Challenge**](http://www.lintcode.com/en/problem/merge-two-sorted-arrays/#challenge)

How can you optimize your algorithm if one array is very large and the other is very small?

这是很标准的merge，从头开始merge

```java
public int[] mergeSortedArray(int[] A, int[] B) {
    if (A == null || B == null) {
        return null;
    }

    int[] res = new int[A.length + B.length];
    if (A.length == 0 && B.length > 0) {
        return B;
    } else if (A.length > 0 && B.length == 0) {
        return A;
    } else if (A.length == 0 && B.length == 0) {
        return res;
    }

    int i = 0;
    int j = 0;
    int cur = 0;
    while (i < A.length && j < B.length) {
        if (A[i] < B[j]) {
            res[cur] = A[i];
            i++;
        } else {
            res[cur] = B[j];
            j++;
        }
        cur++;
    }

    while (i < A.length) {
        res[cur++] = A[i++];
    }

    while (j < B.length) {
        res[cur++] = B[j++];
    }

    return res;
}
```
