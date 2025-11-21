# 2 2分template

```java
public int findPosition(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    int start = 0;
    int end = nums.length - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;

        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (nums[start] == target) {
        return start;
    }

    if (nums[end] == target) {
        return end;
    }

    return -1;
}
```

看了discussion发现其实二分有3种写法，不包括九章上面的那种。然后很多人推荐\[left, right)这种写法。这是c++ lib里面用的方法。这个解释是自己理解了一下这里写的东西，反刍出来的。[https://www.zhihu.com/question/36132386](https://www.zhihu.com/question/36132386)

![](<../.gitbook/assets/image (15).png>)
