# 325 Maximum Size Subarray Sum Equals k

Given an array nums and a target value k, find the maximum length of a subarray that sums to k. If there isn't one, return 0 instead.

**Note:**\
The sum of the entirenumsarray is guaranteed to fit within the 32-bit signed integer range.

**Example 1:**

Givennums=`[1, -1, 5, -2, 3]`,k=`3`,\
return`4`. (because the subarray`[1, -1, 5, -2]`sums to 3 and is the longest)

**Example 2:**

Givennums=`[-2, -1, 2, 1]`,k=`1`,\
return`2`. (because the subarray`[-1, 2]`sums to 1 and is the longest)

**Follow Up:**\
Can you do it in O(n) time?

这题首先算个prefix sum，然后再边loop边找需要的prefix sum的值，如果找到的话，我们算一下长度。hashmap里存的是要找的值，和符合这些值的那些位置。嘛，discuss上有另一种更省时间的写法，也贴上来参考。后来发现，其实不用存所有位置，因为要求的是max，然后现在的i减去靠前面的那些点一定比靠后面的大，所以每个presum只用存第一次找到的位置就ok了。

```java
public int maxSubArrayLen(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int n = nums.length;
    int maxLen = 0;
    int[] preSum = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        preSum[i] = nums[i - 1] + preSum[i - 1];
    }

// <preSum we need to find, first location that need this preSum>
    HashMap<Integer, Integer> hm = new HashMap<>();
    for (int i = 0; i <= n; i++) {
        if (hm.containsKey(preSum[i])) {
            maxLen = Math.max(maxLen, i - hm.get(preSum[i]));
        } 
        if (!hm.containsKey(preSum[i] + k)) {// 如果发现不存在的话，每次都要更新一下hm
            hm.put(preSum[i] + k, i);
        }
    }

    return maxLen;
}
```

```java
public int maxSubArrayLen(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int n = nums.length;
    int maxLen = 0;
    int[] preSum = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        preSum[i] = nums[i - 1] + preSum[i - 1];
    }

    // <preSum we need to find, locations that need this preSum>
    HashMap<Integer, List<Integer>> hm = new HashMap<>();
    for (int i = 0; i <= n; i++) {
        if (hm.containsKey(preSum[i])) {
                List<Integer> tmp = hm.get(preSum[i]);
                for (Integer elem : tmp) {
                    maxLen = Math.max(maxLen, i - elem);
                }
            }

            List<Integer> tmp;
            if (hm.containsKey(preSum[i] + k)) {
                tmp = hm.get(preSum[i] + k);
            } else {
                tmp = new ArrayList<>();
            }
            tmp.add(i);
            hm.put(preSum[i] + k, tmp);
    }

    return maxLen;
}
```

discuss的clean solution：

```java
public int maxSubArrayLen(int[] nums, int k) {
    int sum = 0, max = 0;
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    for (int i = 0; i < nums.length; i++) {
        sum = sum + nums[i];
        if (sum == k) max = i + 1;
        else if (map.containsKey(sum - k)) max = Math.max(max, i - map.get(sum - k));
        if (!map.containsKey(sum)) map.put(sum, i);
    }
    return max;
}
```
