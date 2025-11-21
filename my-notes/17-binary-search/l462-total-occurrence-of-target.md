# L462 Total Occurrence of Target

Given a target number and an integer array sorted in ascending order. Find the total number of occurrences of target in the array.

**Example**

Given`[1, 3, 3, 4, 5]`and target =`3`, return`2`.

Given`[2, 2, 3, 4, 6]`and target =`4`, return`1`.

Given`[1, 2, 3, 4, 5]`and target =`6`, return`0`.

[**Challenge**](http://www.lintcode.com/en/problem/total-occurrence-of-target/#challenge)

Time complexity in O(logn)

分两次二分区找，其实是前两题的结合。找到first position，再找last position，相减就能得到元素个素，记得加一。

```java
public int totalOccurrence(int[] A, int target) {
    if (A == null || A.length == 0) {
        return 0;
    }

    int low = -1;
    int high = -1;

    //find lowest location of target
    int start1 = 0;
    int end1 = A.length - 1;

    while (start1 + 1 < end1) {
        int mid1 = start1 + (end1 - start1) / 2;

        if (A[mid1] == target) {
            end1 = mid1;
        } else if (A[mid1] < target) {
            start1 = mid1;
        } else {
            end1 = mid1;
        }
    }

    if (A[start1] == target) {
        low = start1;
    } else if(A[end1] == target) {
        low = end1;
    }

    //find highest location of target
    int start2 = 0;
    int end2 = A.length - 1;

    while (start2 + 1 < end2) {
        int mid2 = start2 + (end2 - start2) / 2;

        if (A[mid2] == target) {
            start2 = mid2;
        } else if (A[mid2] < target) {
            start2 = mid2;
        } else {
            end2 = mid2;
        }
    }

    if (A[end2] == target) {
        high = end2;
    } else if(A[start2] == target) {
        high = start2;
    }

    // find diff
    if (high == -1 || low == -1) {
        return 0;
    } else {
        return high - low + 1;
    }
}
```
