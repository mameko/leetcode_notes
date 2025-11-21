# L41 Maximum Subarray

Given an array of integers, find a contiguous subarray which has the largest sum.

## Notice

The subarray should contain at least one number.

**Example**

Given the array`[−2,2,−3,4,−1,2,1,−5,3]`, the contiguous subarray`[4,−1,2,1]`has the largest sum =`6`.

[**Challenge**](http://www.lintcode.com/en/problem/maximum-subarray/#challenge)

Can you do it in time complexity O(n)?

这题有3种解法。第一种的note：if you want \[ -3, -2, -1] 's result = 0; set runSum = 0 before set max

else you set max before set runSum = 0

选择记第二种。

```java
// 1. greedy 
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int max = Integer.MIN_VALUE;
    int runSum = 0;

    for (int i = 0; i < nums.length; i++) {
        runSum += nums[i];
        if (runSum > max) {
            max = runSum;
        }
        if (runSum < 0) {
            runSum = 0;
        }
    }

    return max;
}
```

```java
// 2. prefix sum, like gas station, subarray 2, stock 3, subarray diff 
// -- left right tranversal tag
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        int max = Integer.MIN_VALUE;
        int n = nums.length;
        int preSum = 0;
        int minSum = 0;

        for (int i = 0; i < n; i++) {
            preSum = preSum + nums[i];
            max = Math.max(max, preSum - minSum);
            minSum = Math.min(minSum, preSum);
        }

        return max;
    }

// Linked In 考的follow up，要输出start和end的小标
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }

    int max = Integer.MIN_VALUE;
    int n = nums.length;
    int preSum = 0;
    int minSum = 0;

    int start = 0;
    int end = 0;
    for (int i = 0; i < n; i++) { // 加了两个变量取记录index
        preSum += nums[i];
        if (preSum - minSum > max) {
            max = Math.max(preSum - minSum, max);
            end = i;
        }
        if (preSum < minSum) {
            minSum = Math.min(minSum, preSum);
            start = i;
        }
    }
    start++; // 因为prefix sum会多一位，不加加的话就会是个开区间（2，6]，加加以后就会是[3, 6]
    System.out.println("start at : " + start);
    System.out.println("end at : " + end);

    return max;
}
```

```java
// 3. dp - 划分类，subarray 3, stock 4
    public int maxSubArray(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        int n = nums.length;
        int[] global = new int[n];
        int[] local = new int[n];
        global[0] = nums[0];
        local[0] = nums[0];

        for (int i = 1; i < n; i++) {
            // compare prefixSum with 0, if prefixSum < 0, set local to nums[i];
            local[i] = Math.max(nums[i], local[i - 1] + nums[i]);
            global[i] = Math.max(global[i - 1], local[i]);
        }

        return global[n - 1];
    }
```
