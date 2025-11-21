# L459 Closest Number in Sorted Array

Given a target number and an integer array A sorted in ascending order, find the index`i`in A such that A\[i] is closest to the given target.

Return -1 if there is no element in the array.

## Notice

There can be duplicate elements in the array, and we can return any of the indices with same value.

**Example**

Given`[1, 2, 3]`and target =`2`, return`1`.

Given`[1, 4, 6]`and target =`3`, return`1`.

Given`[1, 4, 6]`and target =`5`, return`1`or`2`.

Given`[1, 3, 3, 4]`and target =`2`, return`0`or`1`or`2`.

[**Challenge**](http://www.lintcode.com/en/problem/closest-number-in-sorted-array/#challenge)

O(logn) time complexity.

in fact you can try finding the 1st number that is larger than target. （大于或等于？）\
then check A\[1st] and A\[1st - 1] which one is closer to target, then return

when return 1st lager than target, use this because target may not exist in array

if (A\[start] >= target) {\
return start;\
}\
if (A\[end] >= target) {\
return end;\
}

```java
public int closestNumber(int[] A, int target) {
    if (A == null || A.length == 0) {
        return -1;
    }

    // target is outside the range
    if (A[0] >= target) { 
        return 0;
    }

    if (A[A.length - 1] <= target) {
        return A.length - 1;
    }

    int loc = findFirstLargerThanTarget(A, target);

    // target is inside the array, compare diff between loc & loc - 1
    int diff1 = A[loc] - target;
    int diff2 = target - A[loc - 1];
    if (diff1 < diff2) {
        return loc;
    } else {
        return loc - 1;
    }
}

private int findFirstLargerThanTarget(int[] A, int target) {
    int start = 0;
    int end = A.length - 1;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (A[mid] == target) {
            end = mid;
        } else if (A[mid] < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (A[start] >= target) {
        return start;
    }

    if (A[end] >= target) {
        return end;
    }

    return -1;
}
```
