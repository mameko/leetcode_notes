# L62 Search in Rotated Sorted Array

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e.,`0 1 2 4 5 6 7`might become`4 5 6 7 0 1 2`).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

**Example**

For`[4, 5, 1, 2, 3]`and`target=1`, return`2`.

For`[4, 5, 1, 2, 3]`and`target=0`, return`-1`.

[**Challenge**](http://www.lintcode.com/en/problem/search-in-rotated-sorted-array/#challenge)

O(logN) time

9章的2分模板T:O(logn)，S:O(1)。主要是要半段哪一半有序，然后在那里面找。通过判断target在有序一半或在无序的一半来移动下标。

If duplicate exist, you need to check if A\[mid]==A\[left]

is so, need to check if A\[right]!=A\[mid]

if so, search right

else search both side

```java
public int search(int[] A, int t) {
        if (A == null || A.length == 0) {
            return -1;
        }

        int start = 0;
        int end = A.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (t == A[mid]) {
                return mid;
            } else if (A[start] < A[mid]) {//这里start和mid一段有序
                if (A[start] <= t && t <= A[mid]) {//如果target在那里的话，留
                    end = mid;
                } else {//不然的话，在另外一半找
                    start = mid;
                }
            } else {//同理，这里mid到end有序
                if (t <= A[end] && A[mid] <= t) {
                    start = mid;
                } else {
                    end = mid;
                }
            }
        }

        if (A[start] == t) {
            return start;
        }

        if (A[end] == t) {
            return end;
        }

        return -1;
    }
```
