# 189 Rotate Array

Rotate an array of n elements to the right by k steps.

For example, with n= 7 and k= 3, the array`[1,2,3,4,5,6,7]`is rotated to`[5,6,7,1,2,3,4]`.

**Note:**\
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.

**Hint:**

Could you do it in-place with O(1) extra space?

Related problem:[Reverse Words in a String II](186-reverse-words-in-a-string-ii.md)

很标准的三部翻转，不过要注意处理k，k可能比n大，要mod一下。还有就是注意如果数组长度只有1的话，会越界，而且只有1的话也不用翻，所以直接返回。

```java
public void rotate(int[] nums, int k) {
    if (nums == null || nums.length < 2 || k < 1) {
        return;
    }

    int n = nums.length;
    k = k % n;

    int midStart = n - k;
    reverse(nums, 0, midStart - 1);
    reverse(nums, midStart, n - 1);
    reverse(nums, 0, n - 1);
}

private void reverse(int[] nums, int start, int end) {
    for (int i = start, j = end; i < j; i++, j--) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```
