# L373 Partition Array by Odd and Even

Partition an integers array into odd number first and even number second.

**Example**

Given`[1, 2, 3, 4]`, return`[1, 3, 2, 4]`

[**Challenge**](http://www.lintcode.com/en/problem/partition-array-by-odd-and-even/#challenge)

Do it in-place.

```java
public void partitionArray(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }

    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        while (left <= right && !isEven(nums[left])) {
            left++;
        }

        while (left <= right && isEven(nums[right])) {
            right--;
        }

        if (left <= right) {// check again
            int tmp = nums[left];
            nums[left] = nums[right];
            nums[right] = tmp;
            left++;
            right--;
        }
    }
}

private boolean isEven(int n) {
    if (n % 2 == 0) {
        return true;
    } else {
        return false;
    }
}
```
