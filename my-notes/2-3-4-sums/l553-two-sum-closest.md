# L553 Two Sum Closest

Given an array`nums`o&#x66;_&#x6E;\_integers, find two integers in\_nums\_such that the sum is closest to a given number,\_target_.

Return the difference between the sum of the two integers and the target.

Given array`nums`=`[-1, 2, 1, -4]`, an&#x64;_&#x74;arget_=`4`.

The minimum difference is`1`. (4 - (2 + 1) = 1).

[**Challenge**](http://www.lintcode.com/en/problem/two-sum-closest/#challenge)

Do it in O(nlogn) time complexity.

还是2 pointer，先排序，然后还得用变量keep track of 全局min。这题因为是求closest，所以不能hashmap。

```java
public int twoSumCloset(int[] nums, int target) {
    if (nums == null || nums.length < 2) {
        return Integer.MAX_VALUE;
    }

    Arrays.sort(nums);
    long min = Integer.MAX_VALUE;
    int i = 0;
    int j = nums.length - 1;
    while (i < nums.length && j >= 0 && i < j) {
        long sum = nums[i] + nums[j];
        long diff = Math.abs(target - sum);
        min = Math.min(min, diff);
        if (sum < target) {
            i++;
        } else if (sum > target){
            j--;
        } else {
            return 0;
        }
    }

    return (int)min;
}
```
