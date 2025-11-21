# 74 Search in a 2D Matrix

Write an efficient algorithm that searches for a value in an mxn matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

For example,

Consider the following matrix:

```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```

Given **target**=`3`, return`true`.

可以做2次二分，这里做1次二分。

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return false;
    }

    int n = matrix.length;
    int m = matrix[0].length;

    int start = 0;
    int end = m * n - 1;

    while (start + 1 < end) {
        int mid = start + (end - start) / 2;
        int i = mid / m;
        int j = mid % m;
        if (matrix[i][j] == target) {
            return true;
        } else if (matrix[i][j] < target) {
            start = mid;
        } else {
            end = mid;
        }
    }

    if (matrix[start / m][start % m] == target || 
        matrix[end / m][end % m] == target) {
        return true;
    }

    return false;
}
```
