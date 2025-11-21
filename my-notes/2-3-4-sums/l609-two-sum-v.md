# L609 Two Sum V

Given an array of integers, find how many pairs in the array such that their sum is`less than or equal to`a specific target number. Please return the number of pairs.

**Example**

Given nums =`[2, 7, 11, 15]`, target =`24`.\
Return`5`.\
2 + 7 < 24\
2 + 11 < 24\
2 + 15 < 24\
7 + 11 < 24\
7 + 15 < 25

这题跟Two sumII很像，就是算res的条件有些不一样。

```java
public int twoSum5(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int res = 0;
    Arrays.sort(nums);
    int left = 0;
    int right = nums.length - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum <= target) {
            res += right - left;
            left++;
        } else {
            right--;
        }
    }

    return res;
}
```
