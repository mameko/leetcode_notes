# L443 Two Sum II - use 2 pointer to count

Given an array of integers, find how many pairs in the array such that their sum is bigger than a specific target number. Please return the number of pairs.

**Example**

Given numbers =`[2, 7, 11, 15]`, target =`24`. Return`1`. (11 + 15 is the only pair)

[**Challenge**](http://www.lintcode.com/en/problem/two-sum-ii/#challenge)

Do it in O(1) extra space and O(nlogn) time.

这题跟前面那堆2sum不太一样，求的是大于target的2sum个数。用的还是2 pointer，先排序。在sum>target时更新结果。更新的方法是right - left，因为如果这个right和left能够得到>target的话，那么这个right加上left后面那些（都比left大）的数都一定能比target大。

```java
public int twoSum2(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    Arrays.sort(nums);
    int left = 0;
    int right = nums.length - 1;
    int total = 0;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum > target) {
            //不能放下面的branch来统计，因为会有重复计算
            total += right - left;
            right--;
        } else {
            left++;
        }
    }
    return total;
}
```
