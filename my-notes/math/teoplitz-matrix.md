# 766 Teoplitz Matrix

![](../../.gitbook/assets/ToeplitzMaterix.png)![](../../.gitbook/assets/TeoplitzMatrixInAform.png)

A matrix is\_Toeplitz\_if every diagonal from top-left to bottom-right has the same element.

Now given an`M x N`matrix, return `True` if and only if the matrix i&#x73;_&#x54;oeplitz_.

**Example 1:**

```
Input:
matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
]

Output: True

Explanation:
In the above grid, the diagonals are:
"[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
In each diagonal all elements are the same, so the answer is True.
```

**Example 2:**

```
Input:
matrix = [
  [1,2],
  [2,2]
]

Output: False

Explanation:
The diagonal "[1, 2]" has different elements.
```

**Note:**

1. `matrix`will be a 2D array of integers.
2. `matrix`will have a number of rows and columns in range`[1, 20]`.
3. `matrix[i][j]`will be integers in range`[0, 99]`.

**Follow up:**

1. What if the matrix is stored on disk, and the memory is limited such that you can only load at most one row of the matrix into the memory at once?
2. What if the matrix is so large that you can only load up a partial row into the memory at once?

判断输入的是否一个合法的Tmatrix，第一个follow up只想到load第一行，然后把最后一个去掉，把第二行的第二个load进来，如此类推地比较。第二个follow up感觉要overlap一些，然后一个个三角形覆盖着做。

```java
public boolean isTMatirx(int[][] m) {
      if (m == null || m.length == 0 || m[0].length == 0) {
             return false;
      }

      // check 1 col
      for (int i = 0; i < m.length; i++) {
             if (!checkValidity(i, 0, m)) {
                    return false;
             }
      }

      // check 1 row
      for (int j = 1; j < m[0].length; j++) {
             if (!checkValidity(0, j, m)) {
                    return false;
             }
      }

      return true;
}

private boolean checkValidity(int i, int j, int[][] m) {
      int target = m[i][j];
      while (i < m.length && j < m[0].length) {
             if (target != m[i][j]) {
                    return false;
             }
             i++;
             j++;
      }
      return true;
}

public static void main(String[] args) {
      G_teoplitzMatrix sol = new G_teoplitzMatrix();
      int[][] m = { { 3, 2, 1 }, { 4, 3, 2 }, { 5, 4, 3 } };
      System.out.println(sol.isTMatirx(m));
}
```
