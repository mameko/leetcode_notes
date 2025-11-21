# 747 Largest Number At Least Twice of Others

In a given integer array`nums`, there is always exactly one largest element.

Find whether the largest element in the array is at least twice as much as every other number in the array.

If it is, return the **index** of the largest element, otherwise return -1.

**Example 1:**

```
Input: nums = [3, 6, 1, 0]
Output: 1

Explanation:
6 is the largest integer, and for every other number in the array x,
6 is more than twice as big as x.  The index of value 6 is 1, so we return 1.
```

**Example 2:**

```
Input: nums = [1, 2, 3, 4]
Output: -1

Explanation:
4 isn't at least as big as twice the value of 3, so we return -1.
```

**Note:**

1. `nums`will have a length in the range`[1, 50]`.
2. Every`nums[i]`will be an integer in the range`[0, 99]`.

这题一看上去以为很简单，然后发现还是有corner case的。虽然记得[414 Third Maximum Number](414-third-maximum-number.md)大概的做法，但没记全。要好好背下来。一开始还拿着hashmap来存位置，后来发现其实就2个位置，所以就用变量了。T：O(N)， S：O(1)

```java
public int dominantIndex(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    if (nums.length == 1) {
        return 0;
    } 

    // 找最大和第二大的数
    Integer big = null;
    Integer small = null;
    int bigLoc = -1;
    int smallLoc = -1;
    for (int i = 0; i < nums.length; i++) {
        if (big == null || nums[i] > big) {
            small = big;
            big = nums[i];
            bigLoc = i;
        } else if (small == null || nums[i] > small) {
            small = nums[i];
            smallLoc = i;
        }
    }

    // 最后判断，最大的是不是大于等于第二大的两倍，如果是就有解
    if (big >= small * 2) { 
        return bigLoc;
    } else {
        return -1;
    }
}
```
