# L532 Reverse Pairs

For an array A, if i < j, and A \[i] > A \[j], called (A \[i], A \[j]) is a reverse pair.\
return total of reverse pairs in A.

**Example**

Given A =`[2, 4, 1, 3, 5]`,`(2, 1), (4, 1), (4, 3)`are reverse pairs. return`3`

这题brute force做法是两个for循环来数revere pair的对数。这样要O(n^2)时间。这里利用merge sort来变成nlogn。在merge的步骤里，我们记录有多少个数得从本来的位置往前移和移动多少步。这就是我们要求的和。

```java
public long reversePairs(int[] A) {
    if (A == null || A.length == 0) {
        return 0L;
    }

    int[] tmp = new int[A.length];
    return mergeSort(A, 0, A.length - 1, tmp);
}

private long mergeSort(int[] A, int start, int end, int[] tmp) {
    if (start >= end) {
        return 0L;
    }

    int sum = 0;
    int mid = start + (end - start) / 2;
    sum += mergeSort(A, start, mid, tmp);
    sum += mergeSort(A, mid + 1, end, tmp);
    sum += merge(A, start, mid, end, tmp);

    return sum;

}

private long merge(int[] A, int start, int mid, int end, int[] tmp) {
    for (int i = start; i <= end; i++) {
        tmp[i] = A[i];
    }

    int sum = 0;
    int left = start;
    int right = mid + 1;
    int cur = start;

    while (left <= mid && right <= end) {
        if (tmp[left] <= tmp[right]) {
            A[cur++] = tmp[left++];
        } else {
            A[cur++] = tmp[right++];
            sum += mid - left + 1;
        }
    }

    while (left <= mid) {
        A[cur++] = tmp[left++];
    }

    return sum;
}
```
