# 289 Game of Life

According to wikipedia "The**Game of Life**, also known simply as**Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given aboardwithmbyncells, each cell has an initial statelive(1) ordead(0). Each cell interacts with its[eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood)(horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population..
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state.

**Follow up**:

1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

这题感觉不看答案想不出来。如果不用in-place的话，可以另开一个矩阵更新状态。这里只能在原来的上面更新，做法是利用状态转换。这里有4个状态： 0: death -> death，1: live -> live，2: live -> death，3: death -> live。最后把矩阵里的数%2来得到答案，把四 个状态（0， 1， 2，3）变回两个（0， 1）.这题的follow up，无限矩阵的答案是：因为矩阵比较稀疏，可以通过存坐标来解决。例如：（0，2， live）-> 在坐标0，2的地方有个活的cell，这种做法跟cc189里的16.22 Langton's Ant:有点像，都是更新矩阵状态，无限矩阵的处理。

```java
public void gameOfLife(int[][] board) {
    if (board == null || board.length == 0 || board[0].length == 0) {
        return;
    }

    // 8 directions 
    int[] x = {0, 0, 1, -1, 1, -1, 1, -1};
    int[] y = {1, -1, 0, 0, 1, -1, -1, 1};

    int n = board.length;
    int m = board[0].length;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            int cnt = 0;// cnt alive neighbours

            for (int k = 0; k < 8; k++) {
                int ni = i + x[k];
                int nj = j + y[k];

                if (inBound(ni, nj, board) && (board[ni][nj] == 1 || board[ni][nj] == 2)) {
                    cnt++;
                }
            }

            /*
                0: death -> death
                1: live  -> live
                2: live  -> death
                3: death -> live
            */
            // if > 2 or > 3 neighours, live cell -> death
            if ((cnt > 3 || cnt < 2) && (board[i][j] == 1)) {
                board[i][j] = 2;
            }

            // if we have exact 3 neighbours, death cell -> alive
            if (cnt == 3 && board[i][j] == 0) {
                board[i][j] = 3;
            }
        }
    }

     for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            board[i][j] = board[i][j] % 2;
        }
     }
}

private boolean inBound(int row, int col, int[][] board) {
    if (row < 0 || col < 0 || row >= board.length || col >= board[0].length) {
        return false;
    }
    return true;
}
```
