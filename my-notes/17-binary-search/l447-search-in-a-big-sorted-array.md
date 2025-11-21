# L447 Search in a Big Sorted Array

Given a big sorted array with positive integers sorted by ascending order. The array is so big so that you can not get the length of the whole array directly, and you can only access the kth number by`ArrayReader.get(k)`(or ArrayReader->get(k) for C++). Find the first index of a target number. Your algorithm should be in O(log k), where k is the first index of the target number.

Return -1, if the number doesn't exist in the array.

## Notice

If you accessed an inaccessible index (outside of the array), ArrayReader.get will return`2,147,483,647`.

**Example**

Given`[1, 3, 6, 9, 21, ...]`, and target =`3`, return`1`.

Given`[1, 3, 6, 9, 21, ...]`, and target =`4`, return`-1`.

[**Challenge**](http://www.lintcode.com/en/problem/search-in-a-big-sorted-array/#challenge)

O(log k), k is the first index of the given target number.

```java
// leetcode版
public int search(ArrayReader reader, int target) {
    if (reader == null) {
        return -1;
    }

    int end = 1;
    while (reader.get(end) < target) {
        end = end * 2;
    }

    int start = 0;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (reader.get(mid) == target) {
            return mid; // 只在没有重复的数组里works，应该参照下面的版本
        } else if (reader.get(mid) < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (reader.get(start) == target) {
        return start;
    } else if (reader.get(end) == target) {
        return end;
    } else {
        return -1;
    }
}

// lintcode版
public int searchBigSortedArray(ArrayReader reader, int target) {
    if (reader == null || target < 1){
        return -1;
    }

    int loc  = 1;
    while (reader.get(loc) < target) {
        loc = loc * 2;
    }

    int start = 0;
    int end = loc;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (reader.get(mid) == target) {
            end = mid;
        } else if (reader.get(mid) < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (reader.get(start) == target) {
        return start;
    }

    if (reader.get(end) == target) {
        return end;
    }

    return -1;
}
```
