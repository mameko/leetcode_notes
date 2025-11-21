# 268 Missing Number

Given an array containingndistinct numbers taken from`0, 1, 2, ..., n`, find the one that is missing from the array.

For example,\
Givennums=`[0, 1, 3]`return`2`.

**Note**:\
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

```java
    public int missingNumber(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        int n = nums.length;
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
        }


        return n * (n + 1) / 2 - sum;
    }
```
