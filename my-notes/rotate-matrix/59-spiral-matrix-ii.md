# 59 Spiral Matrix II

Given an integern, generate a square matrix filled with elements from 1 ton2in spiral order.

For example,\
Givenn=`3`,

You should return the following matrix:

```
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

这题因为是n×n所以不用判断只有一行或一列的情况，而且是刚好能填完的。用k记录现在要填的数字，然后用4个变量top，left，bottom，right来记录现在填到哪行哪列了。

```java
public int[][] generateMatrix(int n) {
    int k = 1;// number to be put in
    int top = 0;
    int left = 0;
    int bottom = n - 1;
    int right = n - 1;
    int[][] res = new int[n][n];

    while (k <= n * n) {
        // fill top going right
        for (int i = left; i <= right; i++) {
            res[top][i] = k;
            k++;
        }
        top++;

        // fill right going down
        for (int i = top; i <= bottom; i++) {
            res[i][right] = k;
            k++;
        }
        right--;

        // fill bottom going left, until "left"
        for (int i = right; i >= left; i--) {
            res[bottom][i] = k;
            k++;
        }
        bottom--;

        // fill left going up, until "top"
        for (int i = bottom; i >= top; i--) {
            res[i][left] = k;
            k++;
        }
        left++;
    }

    return res;
}
```
