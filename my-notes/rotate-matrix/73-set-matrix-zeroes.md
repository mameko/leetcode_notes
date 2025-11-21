# 73 Set Matrix Zeroes

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.

这题因为要in place所以难。我们把要设为0的行和列都记录在第一行第一列上面。然而（0， 0）这个位置只能记录第一行或第一列，所以要另开一个variable把另外一个记录下来。这里选择用变量col1st来记录第一列是否应该设0。这里得注意顺序，首先把col1st设好，然后把（0， 0）设好，然后再把第一行第一列设好。当我们把矩阵过了一遍，设好第一行第一列和col1st以后，我们在loop一遍矩阵来把中间的0填上。

```java
public void setZeroes(int[][] m) {
        if (m == null || m.length == 0 || m[0].length == 0) {
        return;
    }

    int col1st = 1;
    for (int i = 0; i < m.length; i++) {
        if (m[i][0] == 0) {
            col1st = 0;
        }
    }

    for (int j = 0; j < m[0].length; j++) {
        if (m[0][j] == 0) {
            m[0][0] = 0;
        }
    }



    for (int i = m.length - 1; i > 0; i--) {
        for (int j = m[0].length - 1; j > 0; j--) {
            if (m[i][j] == 0) {
                m[i][0] = 0;
                m[0][j] = 0;

            }
        }
    }

    for (int i = m.length - 1; i >= 0; i--) {
        for (int j = m[0].length - 1; j > 0; j--) {
            if (m[i][0] == 0 || m[0][j] == 0) {
                m[i][j] = 0;
            }
        }
    }

    if (col1st == 0) {
        for (int i = 0; i < m.length; i++) {
            m[i][0] = 0;
        }
    }
}
```
