# L63 Search in Rotated Sorted Array II

Follow up for L62

What if **duplicates** are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

**Example**

Given`[1, 1, 0, 1, 1, 1]`and target =`0`, return`true`.\
Given`[1, 1, 1, 1, 1, 1]`and target =`0`, return`false`.

这题跟上题很像，不过得去重。

```java
public boolean search(int[] A, int target) {
        if (A == null || A.length == 0) {
            return false;
        }

        int start = 0;
        int end = A.length - 1;

        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (A[mid] == target) {
                return true;
            } else if (A[start] < A[mid]) {//这里注意，如果是start跟mid比较
                if (A[start] <= target && target <= A[mid]) {
                    end = mid;
                } else {
                    start = mid;
                }
            } else if (A[mid] < A[start]) {//这也要比较同样的东西
                if (A[mid] <= target && target <= A[end]) {
                    start = mid;
                } else {
                    end = mid;
                }
            } else {//然后呢，这里要移动同样的东西
                start++;
            }
        }

        if (A[start] == target || A[end] == target) {
            return true;
        }

        return false;
    }
```
