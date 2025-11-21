# 48 Rotate Image

You are given annxn2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

Follow up:\
Could you do this in-place?

其实根据亚麻那题rotate不是方形矩阵的方法，可以有更容易记住的解法。（顺时针的话，先上下翻转，再转置；逆时针的话，先左右翻转，再转置）

```java
public void rotate(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return;
    }

    int n = matrix.length;
    for (int i = 0; i < n / 2; i++) { // 只处理上半个矩阵
        for (int j = 0; j < matrix[0].length; j++) {
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[n - 1 - i][j];
            matrix[n - 1 - i][j] = tmp;
        }
    }

    for (int i = 0; i < n; i++) { // 只处理右上半个三角
        for (int j = i; j < matrix[0].length; j++) {
            int tmp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = tmp;
        }
    }
}
```

主要难在要in-place。而in-place难在下标控制。这里用的是一层一层地rotate。然后每一层里一格一格地rotate。只用一个额外变量，所以是inplace。

```java
public void rotate(int[][] m) {
        if (m == null || m.length == 0 || m.length == 1) {
        return;
    }

    int n = m.length;
    int layers = n / 2;

    for (int l = 0; l < layers; l++) {
        for (int j = l; j < n - l - 1; j++) {
            int temp = m[l][j];
            m[l][j] = m[n - 1 - j][l];
            m[n - 1 - j][l] = m[n - 1 - l][n - 1 - j];
            m[n - 1 - l][n - 1 - j] = m[j][n - 1 - l];
            m[j][n - 1 - l] = temp;
        }
    }
}
```

如果要逆时针转就用下面这段，基本一样：

```java
public int[][] rotateAnt(int[][] m) {
    int n = m.length;
    for (int i = 0; i < n / 2; i++) {
        for (int j = i; j < n - 1 - i; j++) {
            int temp = m[i][j];
            m[i][j] = m[j][n - 1 - i];
            m[j][n - 1 - i] = m[n - 1 - i][n - 1 - j];
            m[n - 1 - i][n - 1 - j] = m[n - 1 - j][i];
            m[n - 1 - j][i] = temp;
        }
    }
    return m;
}
```
