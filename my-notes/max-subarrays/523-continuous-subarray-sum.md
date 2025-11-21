# 523 Continuous Subarray Sum

Given a list of **non-negative** numbers and a target **integer** k, write a function to check if the array has a continuous subarray of size at least 2 that sums up to the multiple of **k**, that is, sums up to n\*k where n is also an **integer**.

**Example 1:**

```
Input:
 [23, 2, 4, 6, 7],  k=6

Output:
 True

Explanation:
 Because [2, 4] is a continuous subarray of size 2 and sums up to 6.
```

**Example 2:**

```
Input:
 [23, 2, 6, 4, 7],  k=6

Output:
 True

Explanation:
 Because [23, 2, 6, 4, 7] is an continuous subarray of size 5 and sums up to 42.
```

**Note:**

1. The length of the array won't exceed 10,000.
2. You may assume the sum of all the numbers is in the range of a signed 32-bit integer.

这题的难点是处理k = 0的情况。因为这时候%k的话会抛异常。处理方法是，当k==0时，我们存preSum到hm里，以后再看到这个preSum的时候，证明我们已经找到一段subarray that add up to 0.(这步完全就是138）如果k != 0，我们存余数。T:O(n), S:O(n)

```java
public boolean checkSubarraySum(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return false;
    }

    int preSum = 0;
    // <nums[i] % val, Location>
    HashMap<Integer, Integer> hm = new HashMap<>();
    hm.put(preSum, 0);
    for (int i = 1; i <= nums.length; i++) {
        preSum += nums[i - 1];
        int key = k == 0 ? preSum : preSum % k;
        if (hm.containsKey(key)) {
            if (i - hm.get(key) > 1) {
                return true;
            }
        } else {
            hm.put(key, i);
        }
    }

    return false;
}
```
