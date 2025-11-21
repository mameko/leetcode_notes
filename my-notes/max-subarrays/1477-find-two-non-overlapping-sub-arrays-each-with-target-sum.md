# 1477 Find Two Non-overlapping Sub-arrays Each With Target Sum

You are given an array of integers `arr` and an integer `target`.

You have to find **two non-overlapping sub-arrays** of `arr` each with a sum equal `target`. There can be multiple answers so you have to find an answer where the sum of the lengths of the two sub-arrays is **minimum**.

Return _the minimum sum of the lengths_ of the two required sub-arrays, or return `-1` if you cannot find such two sub-arrays.

&#x20;

**Example 1:**

<pre><code><strong>Input: arr = [3,2,2,4,3], target = 3
</strong><strong>Output: 2
</strong><strong>Explanation: Only two sub-arrays have sum = 3 ([3] and [3]). The sum of their lengths is 2.
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: arr = [7,3,4,7], target = 7
</strong><strong>Output: 2
</strong><strong>Explanation: Although we have three non-overlapping sub-arrays of sum = 7 ([7], [3,4] and [7]), but we will choose the first and third sub-arrays as the sum of their lengths is 2.
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: arr = [4,3,2,6,2,3,4], target = 6
</strong><strong>Output: -1
</strong><strong>Explanation: We have only one sub-array of sum = 6.
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= arr.length <= 105`
* `1 <= arr[i] <= 1000`
* `1 <= target <= 108`

这题想了好几天，然后看了提示，还是不会。虽然提示已经基本把做法描述好了。逃避现实一个月之后终于抄了答案，但已经忘了之前尝试过的思路了。以后还是得及时写呀。

这个做法是，先左到右来一遍prefixSum求min，再右到左来一遍suffixSum求min，最后从左到右过一遍，把前后加起来的min返回就好了。但一直没掌握好的[138](l138-subarray-sum.md)又出来了。这次好像已经懂了。就是，prefixSum可以先把位置存在值里。又或者可以像[437 Path Sum III](../26-tree/437-path-sum-iii.md) 存path的个数。每次如果在prefixSum的hashmap里，找到了现在为止的sum\_so\_far - target的值，证明，那个位置到到现在这个位置那一段subarray，是target的值。举个栗子：

```
                 [3,    2,    2,    4,    3]
  HashMap:
  sum       0     3     5     7     11    14  
  loc      -1     0     1     2     3      4
                  ^
i = 0    prefixSum = 3, map里有 prefixSum（3）- target（3) = 0的key，
         所以(-1, 0)这一段的subarray=target
                        ^
i = 1    prefixSum = 5, map里有 prefixSum（5）- target（3) = 2的key，继续

i = 2    ...
                                           ^
i = 4    prefixSum = 14, map里有 prefixSum（14）- target（3) = 11的key
         所以(3, 4)这一段的subarray=target
```

suffixSum也能用这种方法做，只是首先放进去的是len（最后一个位置）。这个感觉就像普通array类的前缀和那样，多开一位。这里，是138的拓展，138那里的target是0，所以直接找key就好了。最后因为要不overlap的两条subarray，所以找答案的时候，要空出一格。T:O(n), S:O（n）

```java
public int minSumOfLengths(int[] arr, int target) {
    if (arr == null || arr.length == 0) {
        return -1;
    }

    int len = arr.length;
    int best = Integer.MAX_VALUE;
    int[] left = new int[len];
    int sum = 0;
    // <preSum, loc>
    Map<Integer, Integer> preSumMap = new HashMap<>();
    preSumMap.put(0, -1);

    for (int i = 0; i < len; i++) {
        sum = sum + arr[i];
        if (preSumMap.containsKey(sum - target)) {
            best = Math.min(best, i - preSumMap.get(sum - target));
        }

        left[i] = best;
        preSumMap.put(sum, i);
    }

    best = Integer.MAX_VALUE;
    int[] right = new int[len];
    sum = 0;
    // <sufSum, loc>
    Map<Integer, Integer> sufSumMap = new HashMap<>();
    sufSumMap.put(0, len);

    for (int i = len - 1; i >= 0; i--) {
        sum = sum + arr[i];
        if (sufSumMap.containsKey(sum - target)) {
            best = Math.min(best, sufSumMap.get(sum - target) - i);
        }

        right[i] = best;
        sufSumMap.put(sum, i);
    }

    int result = Integer.MAX_VALUE;
    for (int i = 1; i < len; i++) {
        if (left[i - 1] != Integer.MAX_VALUE && right[i] != Integer.MAX_VALUE) {
            result = Math.min(left[i - 1] + right[i], result);
        }
    }

    return result == Integer.MAX_VALUE ? -1 : result;
}
```

