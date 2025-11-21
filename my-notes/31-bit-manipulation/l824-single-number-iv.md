# L824 Single Number IV

## L824 Single Number IV

## Description

Give an array, all the numbers appear twice except one number which appears once and _all the numbers which appear twice are next to each other_. Find the number which appears once.

* `1 <= nums.length < 10^4`
* In order to limit the time complexity of the program, your program will run`10^5`times.

## Example

Given nums =`[3,3,2,2,4,5,5]`, return`4`.

```
Explanation:
4 appears only once.
```

Given nums =`[2,1,1,3,3]`, return`2`.

```
Explanation:
2 appears only once.
```

这题呢，通过Single Number 1的解法就可以O(n)了。但这题有个特性，因为相同的数字会相邻。我们其实可以二分地找这个多出来数字的位置。这题好像另外在cc189见过的某题，也是通过这种特性找位置。那题好像是在一个排序数组里找一个不按顺序的？忘了，反正就是，那个数字如果出现在左半，那么左边数字个数会是基数，如果落在右边，那么左边个数会是偶数。这题并没有自己写，只把九章的答案抄下了而已，不过思路很熟悉。注意：这里不是完全相同的模板

```java
public int getSingleNumber(int[] nums) {
    // Write your code here
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int mid = (left + right) / 2;
        if (nums[mid] == nums[mid - 1]) {
            if ((mid - left + 1) % 2 == 1) {
                right = mid - 2;
            } else {
                left = mid + 1;
            }
        } else if (nums[mid] == nums[mid + 1]) {
            if ((right - mid + 1) % 2 == 1) {
                left = mid + 2;
            } else {
                right = mid - 1;
            }
        } else {
            return nums[mid];
        }
    }
    return nums[left];
}
```

```java
public int getSingleNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
        return Integer.MIN_VALUE;
    }

    int res = 0;
    for (Integer num : nums) {
        res ^= num;
    }

    return res;
}
```
