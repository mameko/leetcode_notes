# L585 maximum Number in Mountain Sequence

Given a mountain sequence of`n`integers which increase firstly and then decrease, find the mountain top.

**Example**

Given`nums`=`[1, 2, 4, 8, 6, 3]`return`8`\
Given`nums`=`[10, 9, 8, 7]`, return`10`

感觉跟peak element很像。

```java
public int mountainSequence(int[] nums) {
    if (nums == null || nums.length == 0) {
        return Integer.MIN_VALUE;
    }

    if (nums.length == 1) {
        return nums[0];
    }

    if (nums[0] > nums[1]) {
        return nums[0];
    }

    if (nums[nums.length - 2] < nums[nums.length - 1]) {
        return nums[nums.length - 1];
    }

    int start = 1;
    int end = nums.length - 2;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;

        if (nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) {
            return nums[mid];
        } else if (nums[mid] > nums[mid - 1] && nums[mid] < nums[mid + 1]) {
            start = mid;
        } else {
            end = mid;
        }
    }
}
```
