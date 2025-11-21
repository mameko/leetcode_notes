# 561 Array Partition I

Given an array of**2n**integers, your task is to group these integers into**n**pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

**Example 1:**

```
Input: [1,4,3,2]
Output: 4

Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
```

**Note:**

1. **n** is a positive integer, which is in the range of \[1, 10000].
2. All the integers in the array will be in the range of \[-10000, 10000].

这题第一个想到的方法是排序然后把双数位的数加起来。因为要min，所以相邻两个取min能取到最大和。具体数学证明不会。因为这是two pointer题里的，所以一看到就排序了。T: O(NlogN), S O(1)

```java
public int arrayPairSum(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    Arrays.sort(nums);
    int res = 0;
    for (int i = 0; i < nums.length; i += 2) {
        res += nums[i];
    }
    return res;
}
```

另外在solution里看到一种T: O(N), S: O(N)的解法，不明觉厉。因为总数就只有2万个数字，所以建一个hashmap来数数字。主要是控制每一个数取多少个。举个例子：例如给的nums是{1, 1, 1, 3}，那么1会被取两次。arr长这个样子\[...0, 0, 3, 0, 1, 0, 0, ...], (1有3个，3 有一个）

```java
public int arrayPairSum(int[] nums) {
    int[] arr = new int[20001];
    int lim = 10000;
    for (int num: nums)
        arr[num + lim]++;
    int d = 0, sum = 0;
    for (int i = -10000; i <= 10000; i++) {
        sum += (arr[i + lim] + 1 - d) / 2 * i;
        d = (2 + arr[i + lim] - d) % 2;
    }
    return sum;
}
```
