# 1646 Get Maximum in Generated Array

You are given an integer `n`. An array `nums` of length `n + 1` is generated in the following way:

* `nums[0] = 0`
* `nums[1] = 1`
* `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`
* `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`

Return _the **maximum** integer in the array_ `nums`​​​.

**Example 1:**

```
Input: n = 7
Output: 3
Explanation: According to the given rules:
  nums[0] = 0
  nums[1] = 1
  nums[(1 * 2) = 2] = nums[1] = 1
  nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
  nums[(2 * 2) = 4] = nums[2] = 1
  nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
  nums[(3 * 2) = 6] = nums[3] = 2
  nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
Hence, nums = [0,1,1,2,1,3,2,3], and the maximum is 3.
```

**Example 2:**

```
Input: n = 2
Output: 1
Explanation: According to the given rules, the maximum between nums[0], nums[1], and nums[2] is 1.
```

**Example 3:**

```
Input: n = 3
Output: 2
Explanation: According to the given rules, the maximum between nums[0], nums[1], nums[2], and nums[3] is 2.
```

**Constraints:**

*   `0 <= n <= 100`

    You are given an integer `n`. An array `nums` of length `n + 1` is generated in the following way:

    * `nums[0] = 0`
    * `nums[1] = 1`
    * `nums[2 * i] = nums[i]` when `2 <= 2 * i <= n`
    * `nums[2 * i + 1] = nums[i] + nums[i + 1]` when `2 <= 2 * i + 1 <= n`

    Return _the **maximum** integer in the array_ `nums`​​​.

    **Example 1:**

    ```
    Input: n = 7
    Output: 3
    Explanation: According to the given rules:
      nums[0] = 0
      nums[1] = 1
      nums[(1 * 2) = 2] = nums[1] = 1
      nums[(1 * 2) + 1 = 3] = nums[1] + nums[2] = 1 + 1 = 2
      nums[(2 * 2) = 4] = nums[2] = 1
      nums[(2 * 2) + 1 = 5] = nums[2] + nums[3] = 1 + 2 = 3
      nums[(3 * 2) = 6] = nums[3] = 2
      nums[(3 * 2) + 1 = 7] = nums[3] + nums[4] = 2 + 1 = 3
    Hence, nums = [0,1,1,2,1,3,2,3], and the maximum is 3.
    ```

    **Example 2:**

    ```
    Input: n = 2
    Output: 1
    Explanation: According to the given rules, the maximum between nums[0], nums[1], and nums[2] is 1.
    ```

    **Example 3:**

    ```
    Input: n = 3
    Output: 2
    Explanation: According to the given rules, the maximum between nums[0], nums[1], nums[2], and nums[3] is 2.
    ```

    **Constraints:**

    * `0 <= n <= 100`

其实这题有点迷，一看上去，是不是斐波那契数列？然后再看了看，是不是heap？各种想，怎么省空间，算值。然后一看n最大只是100。那，就真的很简单了，其实不懂这题想考什么。规律从第一个例子里就能看出来。T:O(n), S:O(n)

```java
public int getMaximumGenerated(int n) {
    if (n < 0 || n > 100) {
        return -1;
    }

    if (n < 2) {
        return n;
    }

    int max = 0;
    int[] holder = new int[n + 1];
    holder[0] = 0;
    holder[1] = 1;

    for (int i = 2; i <= n; i++) {
        if (i % 2 == 0) {
            holder[i] = holder[i / 2];
        } else {
            holder[i] = holder[i / 2] + holder[i / 2 + 1];
        }

        max = Math.max(max, holder[i]);
    }

    return max;
}
```
