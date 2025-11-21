# 525 Contiguous Array

Given a binary array, find the maximum length of a contiguous subarray with equal number of 0 and 1.

**Example 1:**

```
Input: [0,1]
Output: 2
Explanation: [0, 1] is the longest contiguous subarray with equal number of 0 and 1.
```

**Example 2:**

```
Input: [0,1,0]
Output: 2
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```

**Note:**&#x54;he length of the given binary array will not exceed 50,000.

这题跟cc189 17.5是同一题。做法是分别数prefix 0和1的个数。然后你会发现这两个数的差在0，1数相同的段里是一样的。所以我们可以又一次用[L138](../max-subarrays/l138-subarray-sum.md)的解法。这里记得把其实位置设成（0，-1）

```
例子：   array :　  [0, 1, 0, 1, 1]
         1 cnt :    0  1  1  2  2
         0 cnt :    1  1  2  2  2
         diff  ：0 -1  0  -1  0  0
         loc   :-1  0  1  2   3  4
```

在这个例子中，我们发现max是从一开始到第四个，因为下标从0开始，所以起始位置的下标设为-1，然后我们发现diff相同，一相减就能得到正确长度。

```java
public int findMaxLength(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }

    int max = 0;
    int zeroCnt = 0;
    int oneCnt = 0;
    // <diff, loc>
    HashMap<Integer, Integer> hm = new HashMap<>();
    hm.put(0, -1);
    for (int i = 0; i < nums.length; i++) {
        int cur = nums[i];
        if (cur == 1) {
            oneCnt++;
        } else {
            zeroCnt++;
        }

        int diff = oneCnt - zeroCnt;
        if (hm.containsKey(diff)) {
            int size = i - hm.get(diff);
            max = Math.max(max, size);
        } else {
            hm.put(diff, i);
        }
    }

    return max;
}
```

在tutorial里发现了一个优雅点的写法：

```java
public class Solution {

    public int findMaxLength(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        int maxlen = 0, count = 0;
        for (int i = 0; i < nums.length; i++) {
            count = count + (nums[i] == 1 ? 1 : -1);
            if (map.containsKey(count)) {
                maxlen = Math.max(maxlen, i - map.get(count));
            } else {
                map.put(count, i);
            }
        }
        return maxlen;
    }
}
```
