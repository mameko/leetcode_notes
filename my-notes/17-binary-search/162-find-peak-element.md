# 162 Find Peak Element

## 162 Find Peak Element

A peak element is an element that is greater than its neighbors.

Given an input array where`num[i] ≠ num[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that`num[-1] = num[n] = -∞`.

For example, in array`[1, 2, 3, 1]`, 3 is a peak element and your function should return the index number 2.

**Note:**

Your solution should be in logarithmic complexity.

```java
public int findPeakElement(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    int start = 0;
    int end = nums.length - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] < nums[mid + 1]) {
            start = mid;
        } else {
            end = mid;
        }
    }

    return nums[start] > nums[end] ? start : end;
}
```

## L75 Find Peak Element

There is an integer array which has the following features:

* The numbers in adjacent positions are different.
* A\[0] < A\[1] && A\[A.length - 2] > A\[A.length - 1].

We define a position P is a peek if:

```
A[P] > A[P-1] && A[P] > A[P+1]
```

Find a peak element in this array. Return the index of the peak.

### Notice

The array may contains multiple peeks, find any of them.

**Example**

Given`[1, 2, 1, 3, 4, 5, 7, 6]`

Return index`1`(which is number 2) or`6`(which is number 7)

[**Challenge**](http://www.lintcode.com/en/problem/find-peak-element/#challenge)

Time complexity O(logN)

```java
public int findPeak(int[] A) {
    if (A == null || A.length == 0) {
        return -1;
    }

    int start = 1; 
    int end = A.length - 2;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (A[mid] > A[mid - 1] && A[mid] < A[mid + 1]) {
            // going up, peak at the right
            start = mid;
        } else if (A[mid] < A[mid - 1] && A[mid] > A[mid + 1]) {
            // going down, peak at the left
            end = mid;
        } else if (A[mid] < A[mid - 1] && A[mid] < A[mid + 1]){
            // valley, peak can be on either side
            end = mid;
        } else {
            // the peak
            return mid;
        }
    }

    if (A[start] < A[end]) {
        return end;
    } else {
        return start;
    }
}
```
