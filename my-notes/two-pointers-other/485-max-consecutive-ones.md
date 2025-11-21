# 485 Max Consecutive Ones

Given a binary array, find the maximum number of consecutive 1s in this array.

**Example 1:**

```
Input: [1,1,0,1,1,1]
Output: 3

Explanation:
 The first two digits or the last three digits are consecutive 1s.
 The maximum number of consecutive 1s is 3.
```

**Note:**

* The input array will only contain`0`and`1`.
* The length of input array is a positive integer and will not exceed 10,000

这题的solution不是特别的简洁。不过做法不复杂，就是有指针一直向前找非1，找到了，就开始把左指针向右缩，缩到下一个1开始的地方。最后要注意一下如果最后一个数字是1的话，right是越界的。最后要单独判断一下。

```java
public int findMaxConsecutiveOnes(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int left = 0;        
    int max = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] == 1) {
            continue;
        }           

        while (left < right && nums[left] != 1) {
            left++;
        }
        max = Math.max(right - left, max);
        left = right + 1;
    }

    max = Math.max(max, nums.length - left);
    return max;
}
```

我去，一看discuss发现自己还是想复杂了，就一个指针数就好了，双什么指针呢...囧...

```java
public int findMaxConsecutiveOnes(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int max = 0;
    int cnt = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] == 1) {
            cnt++;
            max = Math.max(max, cnt);
        } else {
            cnt = 0;
        }
    }
    return max;
}
```
