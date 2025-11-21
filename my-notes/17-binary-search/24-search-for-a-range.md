# 34 Search for a Range

Given an array of integers sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order ofO(logn).

If the target is not found in the array, return`[-1, -1]`.

For example,\
Given`[5, 7, 7, 8, 8, 10]`and target value 8,\
return`[3, 4]`.

先找第一个出现的位置，在找最后一个出现的位置。

```java
public int[] searchRange(int[] nums, int target) {
    int[] res = {-1, -1};
    if (nums == null || nums.length == 0) {
        return res;
    }

    int low = -1;
    int high = -1;

    // search 1st pos == target
    int s1 = 0;
    int e1 = nums.length - 1;
    while (s1 + 1 < e1) {
        int m1 = s1 + (e1 - s1) / 2;
        if (nums[m1] == target) {
            e1 = m1;
        } else if (nums[m1] < target) {
            s1 = m1;
        } else {
            e1 = m1;
        }
    }

    if (nums[s1] == target) {
        low = s1;
    } else if (nums[e1] == target) {
        low = e1;
    }

    // search last pos == target
    int s2 = 0;
    int e2 = nums.length - 1;
    while (s2 + 1 < e2) {
        int m2 = s2 + (e2 - s2) / 2;
        if (nums[m2] == target) {
            s2 = m2;
        } else if (nums[m2] < target) {
            s2 = m2;
        } else {
            e2 = m2;
        }
    }

    if (nums[e2] == target) {
        high = e2;
    } else if (nums[s2] == target) {
        high = s2;
    }

    if (high != -1 && low != -1) {
        res[0] = low;
        res[1] = high;
    }

    return res;
}
```
