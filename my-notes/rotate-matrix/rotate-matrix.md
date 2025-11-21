# 亚麻题库的rotate matrix

跟上一题一样，不过给的是一个N × M的矩阵。做法有点不太一样，这次要先转置矩阵取得适合的维数，然后再翻转。其实所谓的翻转也就是把转置矩阵的行列做调整。这个方法对于N×N同样适用。

例子：

```
|1,  2,  3,  4|                   |1,  5,  9|     | 9, 5, 1|      |4, 8, 12|
|5,  6,  7,  8|  == transpose ==> |2,  6, 10|  cw |10, 6, 2|  ccw |3, 7, 11| 
|9, 10, 11, 12|                   |3,  7, 11|     |11, 7, 3|      |2, 6, 10|
                                  |4,  8, 12|     |12, 8, 4|      |1, 5,  9|
```

```java
public int[][] rotateAnt(int[][] mat) {
    int n = mat.length;
    int m = mat[0].length;

    // transpose the matrix to make it in correct dimension
    int[][] transpose = new int[m][n];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            transpose[j][i] = mat[i][j];
        }
    }

    return rotate(transpose, true);
}

private int[][] rotate(int[][] transMat, boolean clockwise) {
    int n = transMat.length;
    int m = transMat[0].length;
    if (clockwise) {// if rotate clock wise 90, reverse every row
        for (int row = 0; row < n; row++) {
            int left = 0;
            int right = m - 1;
            while (left < right) {
                int tmp = transMat[row][left];
                transMat[row][left] = transMat[row][right];
                transMat[row][right] = tmp;
                left++;
                right--;
            }
        }
    } else {// if rotate counter clock wise 90, reverse every col
        for (int col = 0; col < m; col++) {
            int top = 0;
            int bottom = n - 1;
            while (top < bottom) {
                int tmp = transMat[top][col];
                transMat[top][col] = transMat[top][bottom];
                transMat[top][bottom] = tmp;
                top++;
                bottom--;
            }
        }
    }

    return transMat;
}
```
