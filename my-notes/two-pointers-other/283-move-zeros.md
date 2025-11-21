# 283 Move Zeros

Given an array`nums`, write a function to move all`0`'s to the end of it while maintaining the relative order of the non-zero elements.

For example, given`nums = [0, 1, 0, 3, 12]`, after calling your function,`nums`should be`[1, 3, 12, 0, 0]`.

**Note**:

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

这题跟27 Remove Element的框架基本一样。之前那题只把i元素复制到j的位置，就是在else的时候把0换到后面而已。看了tutorial之后补补为神马这里的operation是最佳的。write operation worst case still O(n)，但假设我们的array长这个样子：\[0, 0, 0, 0, 1],我们只需swap第一个和最后一个就ok了。其他的operation都是读。

```java
// 2023.01,再写，移动完直接后面赋值0
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }

    int size = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[size++] = nums[i];
        }
    }

    while (size < nums.length) {
        nums[size++] = 0;
    }       
}

public void moveZeroes(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }

    int n = nums.length;
    int j = 0;
    for (int i = 0; i < n; i++) {
        if (nums[i] == 0) {
            continue;
        } else {
            int tmp = nums[j];
            nums[j++] = nums[i];
            nums[i] = tmp;
        }
    }
}

// 再写
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }

    int size = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            int tmp = nums[size];
            nums[size]= nums[i];
            nums[i] = tmp;
            size++;
        }
    }
}
```

另外这题FB好像有问过变种，说，如果现在不用维持顺序了，怎么才能最少operation。我猜是能做一次快排，pivot会是1，然后把小于1的放左边，大于等于的放右边。
