# 287 Find the Duplicate number

Given an array nums containing n + 1 integers where each integer is between 1 and n(inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Example 1:

```
Input: [1,3,4,2,2]
Output: 2
```

Example 2:

```
Input: [3,1,3,4,2]
Output: 3
```

**Note:**

1. You **must not** modify the array (assume the array is read only).
2. You must use only constant,O(1) extra space.
3. Your runtime complexity should be less than`O(n2).`
4. There is only one duplicate number in the array, but it could be repeated more than once.

~~看了答案，找重复，其实一开始就该想到**set**，脑残了。~~（3年后再看，题目要求不能用extra space，所以不能另外开一个set）最后来一种leetcode solution里的Floyd's Tortoise and Hare (Cycle Detection)。如果只有n个数，那么位置号码与下标一一对应，从0开始找第一个的位置，一直找不会有重复。但现在有n+1，我们会发现有死循环的情况出现。这题其实可以转换成[142 Linked List Cycle II](../18-linkedlist/142-linked-list-cycle-ii.md)的**快慢指针**来做。因为0不在数组里，所以我们从num\[0]开始。p1走一步，p2走两步，然后走到值相同的时候，我们把p1放回前头，然后p1和p2一齐走。最后又走到值相等的时候，我们就得到重复的数字。

```java
public int findDuplicate(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    // slow
    int ptr1 = nums[0];
    // fast
    int ptr2 = nums[0];

    do {
        ptr1 = nums[ptr1];
        ptr2 = nums[nums[ptr2]];            
    } while (ptr1 != ptr2);

    // move fast back to starting point
    ptr2 = nums[0];
    while (ptr1 != ptr2) {
        ptr1 = nums[ptr1];
        ptr2 = nums[ptr2];
    }

    return ptr1;
}
```

~~卧槽，两年后脑残了，排序以后直接找就行了。我特么的2分毛呀~~。（再过一年再看，发现，题目要求不能改array所以不能排序...真心觉得每次重刷都有新发现)

```java
public int findDuplicate(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    Arrays.sort(nums);

    int start = 0;
    int end = nums.length - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        if (nums[mid] >= mid + 1) {
            start = mid;
        } else if (nums[mid] < mid + 1) {
            end = mid;
        } 
    }

    if (start + 1 < nums[start]) {
        return start;
    } else {
        return end;
    }
}
```

这题其实是2分答案，因为数组里的数是从 1 到 n。所以我们2分地猜是哪一个重复了。原因是，如果是无重复的数字排序以后，所对应的位置应该跟数字相同。例如，【1，2，3】，第二个位置的数字，也同样是第二小的数字。所以如果有重复的话，例如，【1，1，2，3】，那么小于第二个位置的数字，就不是第二小了。所以判断的条件是如果小于等于mid位置的数字的数目小于等于mid的话，说明重复的数字在后面一段（mid 到 end）。相反，我们找前面一段。这里婊演了一种诡异的nlogn2分...

```java
public int findDuplicate(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    // binary search on result
    int start = 1;
    int end = nums.length - 1;
    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        // if we have more num <= to mid is less than mid, 
        // we know that the dup is in mid to end
        if (lessOrEqual(mid, nums) <= mid) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (lessOrEqual(start, nums) <= start) {
        return end;
    } else {
        return start;
    }
}

// Counting how many is less than the number at index n
private int lessOrEqual(int n, int[] nums) {
    int res = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] <= n) {
            res++;
        }
    }
    return res;
}
```
