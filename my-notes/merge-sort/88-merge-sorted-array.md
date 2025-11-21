# 88 Merge Sorted Array

Given two sorted integer arraysnums1andnums2, mergenums2intonums1as one sorted array.

**Note:**\
You may assume that nums1 has enough space (size that is greater or equal tom+n) to hold additional elements fromnums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

merge两个数组，数组1有足够的空间装下数组2。注意从后面开始merge。

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    if (nums1 == null || nums2 == null) {
        return;
    }

    int i = m - 1;// num1的下标
    int j = n - 1;// num2的下标
    int cur = m + n - 1;// 结果数组的下标

    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[cur--] = nums1[i--];
        } else {
            nums1[cur--] = nums2[j--];
        }
    }
    //这里不用 while (i >= 0)，因为num1数组的数已经在里面了。
    while (j >= 0) {
            nums1[cur--] = nums2[j--];
    }
}
```
