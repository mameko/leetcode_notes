# L587 Two Sum VI - Unique two sum pairs

Given an array of integers, find how many`unique pairs`in the array such that their sum is equal to a specific target number. Please return the number of pairs.

**Example**

Given nums =`[1,1,2,45,46,46]`, target =`47`\
return`2`

1 + 46 = 47\
2 + 45 = 47

这题是得注意去重，在找到答案以后要跳过相同的数字不断移动指针。

```java
public int twoSum6(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int res = 0;
    Arrays.sort(nums);
    int left = 0;
    int right = nums.length - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) {
            res++;
            left++;
            right--;
            while (nums[left] == nums[left - 1] && left < right) {
                left++;
            }
            while (nums[right] == nums[right + 1] && left < right) {
                right--;
            }
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return res;
}
```
