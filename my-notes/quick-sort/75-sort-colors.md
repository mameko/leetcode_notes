# 75 Sort colors

Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

**Note:**\
You are not suppose to use the library's sort function for this problem.

[click to show follow up.](https://leetcode.com/problems/sort-colors/)

**Follow up:**\
A rather straight forward solution is a two-pass algorithm using counting sort.\
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.

Could you come up with an one-pass algorithm using only constant space?

两种做法，3 way partition 和桶排序，sort color II写了2种。

```java
public void sortColors(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }

    int i = 0;
    int left = 0;
    int right = nums.length - 1;
    int pivot = 1;

    while (i <= right) {
        if (nums[i] < pivot) {
            int tmp = nums[left];
            nums[left] = nums[i];
            nums[i] = tmp;
            left++;
            i++;
        } else if (nums[i] == pivot) {
            i++;
        } else {
            int tmp = nums[right];
            nums[right] = nums[i];
            nums[i] = tmp;
            right--;
        }
    }

    return;
}
```
