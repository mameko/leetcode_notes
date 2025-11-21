# 280 Wiggle Sort

Given an unsorted array`nums`, reorder it **in-place** such that`nums[0] <= nums[1] >= nums[2] <= nums[3]...`.

For example, given`nums = [3, 5, 2, 1, 6, 4]`, one possible answer is`[1, 6, 2, 5, 3, 4]`.

因为允许相等，所以难度不大，只要loop一次把小的放偶数位，大的放奇数位就ok了。

```java
public void wiggleSort(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }

    for (int i = 0; i < nums.length - 1; i++) {
        if ((i % 2 == 0 && nums[i] > nums[i + 1]) ||
            (i % 2 != 0 && nums[i] < nums[i + 1])) {
            swap(i, i + 1, nums);
        }
    }
}

private void swap(int i, int j, int[] nums) {
    int tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}
```
