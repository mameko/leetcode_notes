# L405 Submatrix Sum

Given an integer matrix, find a submatrix where the sum of numbers is zero. Your code should return the coordinate of the left-up and right-down number.

**Example**

Given matrix

```
[
  [1 ,5 ,7],
  [3 ,7 ,-8],
  [4 ,-8 ,9],
]
```

return`[(1,1), (2,2)]`

[**Challenge**](http://www.lintcode.com/en/problem/submatrix-sum/#challenge)

O(n3) time.

这题感觉是[L138 Subarray Sum](https://mameko.gitbooks.io/pan-s-algorithm-notes/content/l138-subarray-sum.html)的二维版。这里算好了prefix matrix，然后枚举上下边界。naive做法是枚举左右边界，然后时间复杂度就会是O(n^4)。这里用了L138的hashmap方法来把左右边界枚举的复杂度降低到O(n)。因为返回是原来matrix的下标，所以h和j要减一（sum matrix多加了一行一列）。

```java
public int[][] submatrixSum(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return null;
    }

    int n = matrix.length;
    int m = matrix[0].length;
    int[][] res = new int[2][2];
    // calculate matrix prefix sum
    int[][] sum = new int[n + 1][m + 1];
    // sum[i][j] - sum from m[0,0] to m[i,j]
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            sum[i][j] = matrix[i - 1][j - 1] + sum[i - 1][j] + sum[i][j - 1] - sum[i - 1][j - 1];
        }
    }

    // try to fix height then loop through width
    // then use subarray sum I
    for (int l = 0; l < n; l++) {
        for (int h = l + 1; h <= n; h++) {
            HashMap<Integer, Integer> hm = new HashMap<>();
            for (int j = 0; j <= m; j++) {
                int diff = sum[h][j] - sum[l][j];
                if (hm.containsKey(diff)) {
                    res[0][0] = l;
                    res[0][1] = hm.get(diff);
                    res[1][0] = h - 1;
                    res[1][1] = j - 1;
                    return res;
                } else {
                    hm.put(diff, j);
                }
            }
        }
    }

    return res;
}
```
