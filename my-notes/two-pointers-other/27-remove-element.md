# 27 Remove Element

Given an array and a value, remove all instances of that value in place and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

**Example:**\
Given input arraynums=`[3,2,2,3]`,val=`3`

Your function should return length = 2, with the first two elements ofnumsbeing 2.

**Hint:**

1. Try two pointers.
2. Did you use the property of "the order of elements can be changed"?
3. What happens when the elements to remove are rare?

用两个指针来处理这种题，嘛，都放到2pointers这章里了。首先用loop through整个array。另外用个size来记录实际长度和删除目标数字后数组的下标。当找到要删除的目标时，不移动size那个下标，只移动i，跳过那个element。如果现在元素不是target元素时，我们把它复制到最后结果数组对应的下标（size）

```java
public int removeElement(int[] nums, int val) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int size = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == val) {
            continue;
        } else {
            nums[size++] = nums[i];
        }
    }

    return size;
}
```
