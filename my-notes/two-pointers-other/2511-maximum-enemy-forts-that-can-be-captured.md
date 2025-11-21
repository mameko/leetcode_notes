# 2511 Maximum Enemy Forts That Can Be Captured

You are given a **0-indexed** integer array `forts` of length `n` representing the positions of several forts. `forts[i]` can be `-1`, `0`, or `1` where:

* `-1` represents there is **no fort** at the `ith` position.
* `0` indicates there is an **enemy** fort at the `ith` position.
* `1` indicates the fort at the `ith` the position is under your command.

Now you have decided to move your army from one of your forts at position `i` to an empty position `j` such that:

* `0 <= i, j <= n - 1`
* The army travels over enemy forts **only**. Formally, for all `k` where `min(i,j) < k < max(i,j)`, `forts[k] == 0.`

While moving the army, all the enemy forts that come in the way are **captured**.

Return _the **maximum** number of enemy forts that can be captured_. In case it is **impossible** to move your army, or you do not have any fort under your command, return `0`_._

&#x20;

**Example 1:**

<pre><code><strong>Input: forts = [1,0,0,-1,0,0,0,0,1]
</strong><strong>Output: 4
</strong><strong>Explanation:
</strong>- Moving the army from position 0 to position 3 captures 2 enemy forts, at 1 and 2.
- Moving the army from position 8 to position 3 captures 4 enemy forts.
Since 4 is the maximum number of enemy forts that can be captured, we return 4.
</code></pre>

**Example 2:**

<pre><code><strong>Input: forts = [0,0,1,-1]
</strong><strong>Output: 0
</strong><strong>Explanation: Since no enemy fort can be captured, 0 is returned.
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= forts.length <= 1000`
* `-1 <= forts[i] <= 1`

这题，要找1和-1或者-1和1之间有多少个0.用同向双指针，先用right每次往右扫一扫。找到一个非0的，就用last来记录一下。然后，如果right又指向非0的数字，判断一下是否跟last是一样的，一样的话，不处理，更新last位置。一样的情况表示我们是1，0，1 or -1，0，0，-1这些情况，所以跳过。最后，如果两个指向的是不同的数字，1，0，0，-1 or 1， 0， -1之类的情况，我们可以算个max。记住代入下标算。T: O(n), S:O(1)

```java
public int captureForts(int[] forts) {
    if (forts == null || forts.length == 0) {
        return 0;
    }

    int max = 0;
    int last = -1;
    int right = 0;
    while (right < forts.length) {
        if (forts[right] == 0) {
            right++;
            continue;
        }

        if (last == -1) {
            last = right;
            right++;  
            continue;
        }

        if (forts[last] == forts[right]) {
            last = right;
            right++;  
            continue;
        } else {                
            max = Math.max(right - last - 1, max);
            last = right;
            right++;  
        }                      
    }
    return max;
}
```
