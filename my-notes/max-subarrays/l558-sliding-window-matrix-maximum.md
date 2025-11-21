# L558 Sliding Window Matrix Maximum

Given an array of`n * m`matrix, and a moving matrix window (size`k * k`), move the window from top left to botton right at each iteration, find the maximum number inside the window at each moving.\
Return`0`if the answer does not exist.

**Example**

For matrix

```
[
  [1, 5, 3],
  [3, 2, 1],
  [4, 1, 9],
]
```

The moving window size k =`2`.\
return 13.

At first the window is at the start of the array like this

```
[
  [|1, 5|, 3],
  [|3, 2|, 1],
  [4, 1, 9],
]
```

,get the sum`11`;\
then the window move one step forward.

```
[
  [1, |5, 3|],
  [3, |2, 1|],
  [4, 1, 9],
]
```

,get the sum`11`;\
then the window move one step forward again.

```
[
  [1, 5, 3],
  [|3, 2|, 1],
  [|4, 1|, 9],
]
```

,get the sum`10`;\
then the window move one step forward again.

```
[
  [1, 5, 3],
  [3, |2, 1|],
  [4, |1, 9|],
]
```

,get the sum`13`;\
SO finally, get the maximum from all the sum which is`13`.

[**Challenge**](http://www.lintcode.com/en/problem/sliding-window-matrix-maximum/#challenge)

O(n^2) time.

这题又是prefix sum matrix的应用。在计算实际matrix时，注意是从i，j = 0，0 到= （n-k, m-k）。

```java
public int maxSlidingMatrix(int[][] matrix, int k) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return 0;
    }

    int n = matrix.length;
    int m = matrix[0].length;

    int[][] preSum = new int[n + 1][m + 1];
    // calculate prefix sum matrix
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            preSum[i][j] = preSum[i - 1][j] + preSum[i][j - 1] + matrix[i - 1][j - 1] - preSum[i - 1][j - 1];
        }
    }

    int max = Integer.MIN_VALUE;
    // find window matrix sum
    for (int i = 0; i <= n - k; i++) {
        for (int j = 0; j <= m - k; j++) {
            // (0, 0) -> (1, 1) : (2, 2) + (0, 0) - (0, 2) - (2, 0)
            int i2 = i + k;
            int j2 = j + k;

            int sum = preSum[i2][j2] + preSum[i][j] - preSum[i2][j] - preSum[i][j2];
            max = Math.max(max, sum);
        }
    }

    return max == Integer.MIN_VALUE ? 0 : max;
}
```
