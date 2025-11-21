# 259 3 Sum Smaller

Given an array of n integers nums and a target, find the number of index triplets`i, j, k`with`0 <= i < j < k < n`that satisfy the condition`nums[i] + nums[j] + nums[k] < target`.

For example, givennums=`[-2, 0, 1, 3]`, andtarget= 2.

Return 2. Because there are two triplets which sums are less than 2:

```
[-2, 0, 1]
[-2, 0, 3]
```

**Follow up:**\
Could you solve it inO(n2) runtime?

这题思路跟L2sum II很像，只是换成了3sum而已。只要注意判断left和right的移动条件跟+res的条件就ok了。

```java
public int threeSumSmaller(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    Arrays.sort(nums);
    int n = nums.length;
    int res = 0;
    for (int i = 0; i < n; i++) {
        // 2 pointers
        int left = i + 1;
        int right = n - 1;
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            if (sum < target) {
                res += right - left;
                left++;
            } else {
                right--;
            }
        }
    }

    return res;
}
```
