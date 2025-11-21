# 54 Spiral Matrix

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

For example,\
Given the following matrix:

```
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
```

You should return`[1,2,3,6,9,8,7,4,5]`.

这题因为是n×m的，所以要注意输出最后一行或一列的情况。个人觉得这个做法最容易理解，所以背了。用n和m来记录现在打印到第几层了，因为是层数，所以每次减2。用x和y分别来作为下标来跑一圈（一层）。注意圈圈里跑的时候循环条件要< n -1/ < m -1，这是因为最后一个元素会在转弯后打印，因为用完后再++所以会直接到那个位置，然后拐弯继续打。T:O(m\*n), S:O(1)

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> res = new ArrayList<>();
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return res;
    }

    int n = matrix.length;
    int m = matrix[0].length;

    // initialze to top left corner, every round x & y will start at the top left of that layer
    int x = 0;// going down
    int y = 0;// going right

    while (n > 0 && m > 0) {
        if (n == 1) {// only one row left
            for (int j = 0; j < m; j++) {
                res.add(matrix[x][y++]);
            }
            break;
        } else if (m == 1) {// only one col left
            for (int i = 0; i < n; i++) {
                res.add(matrix[x++][y]);
            }
            break;
        }

        // left -> right
        for (int j = 0; j < m - 1; j++) {
            res.add(matrix[x][y++]);
        }

        // top -> bottom
        for (int i = 0; i < n - 1; i++) {
            res.add(matrix[x++][y]);
        }

        // right -> left
        for (int j = 0; j < m - 1; j++) {
            res.add(matrix[x][y--]);
        }

        // bottom -> top
        for (int i = 0; i < n - 1; i++) {
            res.add(matrix[x--][y]);
        }

        x++;
        y++;
        m = m - 2;
        n = n - 2;
    }

    return res;

}
```

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> res = new ArrayList<>();
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return res;
    }

    int n = matrix.length;
    int m = matrix[0].length;

    int rowInd = 0;
    int colInd = 0;

    while (n > 0 && m > 0) {
        if (n == 1) {
            for (int j = 0; j < m; j++) {
                res.add(matrix[rowInd][colInd++]);
            }
            break;
        } else if (m == 1) {
            for (int i = 0; i < n; i++) {
                res.add(matrix[rowInd++][colInd]);
            }
            break;
        }

        for (int j = 0; j < m - 1; j++) {
            res.add(matrix[rowInd][colInd++]);
        }

        for (int i = 0; i < n - 1; i++) {
            res.add(matrix[rowInd++][colInd]);
        }

        for (int j = 0; j < m - 1; j++) {
            res.add(matrix[rowInd][colInd--]);
        }

        for (int i = 0; i < n - 1; i++) {
            res.add(matrix[rowInd--][colInd]);
        }

        rowInd++;
        colInd++;
        m = m - 2;
        n = n - 2;
    }

    return res;
}
```
