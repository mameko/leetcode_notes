# 35 Search Insert Position

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.\
`[1,3,5,6]`, 5 → 2\
`[1,3,5,6]`, 2 → 1\
`[1,3,5,6]`, 7 → 4\
`[1,3,5,6]`, 0 → 0

这题貌似简单，其实套路很深。要注意2分找的是什么。其实要找的是第一个>= target的位置。最后判断要小心。那么为什么不能用反的条件呢？就是为什么不能找最后\<target的位置呢，因为会返回错的结尾位置。我觉得可能是因为如果要插入的是第一个位置，我们不能减1，-1不是合理的位置。但如果是最后一个位置，我们可以加1，所以不能用反条件。这题虽然都是正数，但这个方法插入负数也是ok的

```java
public int searchInsert(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    // find 1st position >= target
    int start = 0;
    int end = nums.length - 1;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (nums[start] >= target) {
        return start;
    } else if (nums[end] >= target) {
        return end;
    } else {
        return end + 1;
    }
}
```
