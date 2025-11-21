# 209 Minimum Size Subarray Sum

Given an array of **n** positive integers and a positive integer**s**, find the minimal length of a **contiguous** subarray of which the sum ≥**s**. If there isn't one, return 0 instead.

For example, given the array`[2,3,1,2,4,3]`and`s = 7`,\
the subarray`[4,3]`has the minimal length under the problem constraint.

**More practice:**

If you have figured out theO(n) solution, try coding another solution of which the time complexity isO(nlogn).

两年后再做，调了半天才过。发现right的条件控制有点难度。

```java
public int minSubArrayLen(int s, int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int left = 0;
    int right = 0;
    int sum = 0;
    int min = Integer.MAX_VALUE;
    while (right <= nums.length) {// 要控制好下标。不然会漏了最后一个
        if (sum >= s) {                               
            min = Math.min(min, right - left);                
            sum -= nums[left++];
        } else {
            if (right == nums.length) break; // 这时候只有左下标可以增加，如果到这个branch证明我们已经做完了
            sum += nums[right++];
        }
    }

    return min == Integer.MAX_VALUE ? 0 : min;
}
```

个人觉得这题比较像sliding window，首先把有指针移动到符合条件的地方，然后再缩小左指针，缩小时记录结果。

```java
public int minSubArrayLen(int s, int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int min = Integer.MAX_VALUE;
    int right = 0;
    int sum = 0;
    for (int left = 0; left < nums.length; left++) {
        while (right < nums.length && sum < s) {
            sum += nums[right++];
        }

        if (sum >= s) {
            int len = right - left;
            min = Math.min(min, len);
            sum = sum - nums[left];
        } 
    }

    return min == Integer.MAX_VALUE ? 0 : min;
}
```

在Lint上的写法更像模板

```java
public int minimumSize(int[] nums, int s) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    int i = 0;
    int j = 0;
    int len = nums.length;
    int min = Integer.MAX_VALUE;
    int sum = nums[0];
    while (i <= j && j < len) {
        if (sum < s) {
            j++;
            if (j >= len) {
                break;
            }
            sum = sum + nums[j];
        } else {
            min = Math.min(min, j - i + 1);
            sum = sum - nums[i];
            i++;
        }
    }

    return min == Integer.MAX_VALUE ? -1 : min;
}
```
