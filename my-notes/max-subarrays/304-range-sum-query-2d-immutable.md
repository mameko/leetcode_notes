# 304 Range Sum Query 2D - immutable

Given a 2D matrixmatrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1,col1) and lower right corner (row2,col2).

![Range Sum Query 2D](https://leetcode.com/static/images/courses/range_sum_query_2d.png)\
The above rectangle (with the red border) is defined by (row1, col1) =**(2, 1)**&#x61;nd (row2, col2) =**(4, 3)**, which contains sum =**8**.

**Example:**

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```

**Note:**

1. You may assume that the matrix does not change.
2. There are many calls to sumRegion function.
3. You may assume that row1 ≤ row2 and col1 ≤ col2.

这题就是求二维的prefix sum。求prefix Sum的方法是，左下+右上+这个值 - 左上。查询时是：右下的（i2+1，j2+1）- （i1，j2 +1）- （i2 + 1， j1）+ (i1, j1)

```java
int[][] preSumM;
public NumMatrix(int[][] matrix) {
    if (matrix.length == 0 || matrix[0].length == 0) {
        return;
    }

    int n = matrix.length;
    int m = matrix[0].length;

    preSumM = new int[n + 1][m + 1];
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            preSumM[i][j] = preSumM[i - 1][j] + preSumM[i][j - 1] + matrix[i - 1][j - 1] - preSumM[i - 1][j - 1];
        }
    }
}

public int sumRegion(int row1, int col1, int row2, int col2) {
    return preSumM[row2 + 1][col2 + 1] + preSumM[row1][col1] - preSumM[row1][col2 + 1] - preSumM[row2 + 1][col1];
}
```
