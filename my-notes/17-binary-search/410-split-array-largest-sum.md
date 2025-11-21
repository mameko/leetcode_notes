# 410 Split Array Largest Sum

Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

**Note:**\
If n is the length of array, assume the following constraints are satisfied:

* 1 ≤ n ≤ 1000
* 1 ≤ m ≤ min(50, n )

**Examples:**

```
Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.The best way is to split it 
into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.
```

这题一开始以为是区间类的DP，拆来拆去，没想到dp数组记录什么。然后看了一下网上的解法，好像是二维dp但O(n^3)。感觉像划分类。然后顺便看了一下答案，说能二分，然后就想到二分答案了。这题要注意越界，所以用long。另外，二分找的是第一个满足条件（min）的值。还有，那个判断函数比较难写对，调了半天。

```java
public int splitArray(int[] nums, int m) {
    if (m < 1 || nums == null || nums.length == 0) {
        return 0;
    }

    long start = 0L;
    long end = 0L;
    // calculate max range
    for (Integer num : nums) {
        end += num;
    }

    // binary search for result
    while (start + 1 < end) {
        long mid = start + (end - start) / 2;
        if (splitM(mid, nums, m)) {
            end = mid;
        } else {
            start = mid;
        }
    }

    if (splitM(start, nums, m)) {
        return (int)start;
    } else {
        return (int)end;
    }
}

private boolean splitM(long target, int[] A, int len) {
        int count = 0;
        int i = 0;
        while (i < A.length) {
            long sum = 0;
            boolean ended = false;
            while (sum <= target) {
                if (i < A.length) {
                    sum += A[i++];
                } else {
                    ended = true;
                    break;
                }
            }
            count++;
            if (count > len) {
                return false;
            }
            if (!ended) {
                i--;
            }
        }

        return true;
    }
```
