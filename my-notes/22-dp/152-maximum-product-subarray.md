# 152 Maximum Product Subarray

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

**Example**

For example, given the array`[2,3,-2,4]`, the contiguous subarray`[2,3]`has the largest product =`6`.

因为max value有可能来自min × min，所以我们需要多开一条array来记录min

```java
public int maxProduct(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int[] max = new int[nums.length];
    int[] min = new int[nums.length];
    max[0] = nums[0];
    min[0] = nums[0];
    int res = nums[0];
    for (int i = 1; i < nums.length; i++) {
        max[i] = nums[i];
        min[i] = nums[i];
        if (nums[i] > 0) {
            max[i] = Math.max(max[i], max[i - 1] * nums[i]);
            min[i] = Math.min(min[i], min[i - 1] * nums[i]);
        } else if (nums[i] < 0){
            max[i] = Math.max(max[i], min[i - 1] * nums[i]);
            min[i] = Math.min(min[i], max[i - 1] * nums[i]);
        }

        res = Math.max(res, max[i]);
    }
    return res;
}
```
