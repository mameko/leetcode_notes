# 37 Sudoku solver

Write a program to solve a Sudoku puzzle by filling the empty cells.

Empty cells are indicated by the character`'.'`.

You may assume that there will be only one unique solution.

![](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

A sudoku puzzle...

![](http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

...and its solution numbers marked in red.

这题跟N Queen的解法很像，只是判断合法的条件不一样，还有就是加结果时不一样，N皇后用个tmplist一层一层找，找完一层加一层结果。这个sudoku得改原来的board，然后一格一格地找。T:O(9 ^ empty spots), S:O(empty spots)，每个空格有9中选择，递归深度是要填的格数。

```java
public void solveSudoku(char[][] board) {
    if (board == null || board.length != 9 || board[0].length != 9) {
        return;
    }

    // use 3 matrix to mark what number is already used
    boolean[][] rowUsed = new boolean[9][9];
    boolean[][] colUsed = new boolean[9][9];
    boolean[][] cubUsed = new boolean[9][9];

    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] != '.') {
                int num = board[i][j] - 1 - '0';
                rowUsed[i][num] = true;
                colUsed[num][j] = true;
                cubUsed[3 * (i / 3) + (j / 3)][num] = true;
            }
        }
    }

    dfsSearch(rowUsed, colUsed, cubUsed, board, 0, 0);

}

private boolean dfsSearch(boolean[][] rowUsed, boolean[][] colUsed, boolean[][] cubUsed, char[][] board, int i, int j) {
    // we finished filling the board
    if (i == 9) {
        return true;
    }

    // we complete 1 row, go on to next row
    if (j >= 9) {
        return dfsSearch(rowUsed, colUsed, cubUsed, board, i + 1, 0);
    }

    // find a blank
    if (board[i][j] == '.') {
        // try to fill that blank
        for (int num = 0; num < 9; num++) {
            if (rowUsed[i][num] || colUsed[num][j] || cubUsed[3 * (i / 3) + (j / 3)][num]) {
                continue;// skip invalid numbers
            } else {
                // fill the blank fill the visited matrixs                    
                board[i][j] = (char)(num + 1 + '0');
                rowUsed[i][num] = true;
                colUsed[num][j] = true;
                cubUsed[3 * (i / 3) + (j / 3)][num] = true;
                // and go to next cell
                boolean suc = dfsSearch(rowUsed, colUsed, cubUsed, board, i, j + 1);

                if (!suc) {
                    // if not success we backtrack
                    board[i][j] = '.';
                    rowUsed[i][num] = false;
                    colUsed[num][j] = false;
                    cubUsed[3 * (i / 3) + (j / 3)][num] = false;
                } else {
                    // if success we return true
                    return true;
                }
            }
        }

     } else {
         // if it's not a blank we go on to next cell
         return dfsSearch(rowUsed, colUsed, cubUsed, board, i, j + 1);
     }

    // if we can't find a solution, we return false;
    return false;
}
```

```java
public void solveSudoku(char[][] board) {
    if (board == null || board.length != 9 || board[0].length != 9) {
        return;
    }

    boolean[][] rowUsed = new boolean[9][9];
    boolean[][] colUsed = new boolean[9][9];
    boolean[][] cubUsed = new boolean[9][9];

    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] != '.') {
                int num = board[i][j] - '0' - 1;
                rowUsed[i][num] = true;
                colUsed[num][j] = true;
                cubUsed[3 * (i / 3) + j / 3][num] = true;
            }
        }
    }

    dfs(rowUsed, colUsed, cubUsed, 0, 0, board);
}

private boolean dfs(boolean[][] rowUsed, boolean[][] colUsed, boolean[][] cubUsed, int i, int j, char[][] board) {
    if (i >= 9) {
        return true;
    }

    if (j >= 9) {
        return dfs(rowUsed, colUsed, cubUsed, i + 1, 0, board);
    }

    for (int num = 0; num < 9; num++) {
        if (board[i][j] == '.') {
            if (rowUsed[i][num] || colUsed[num][j] || cubUsed[3 * (i / 3) + j / 3][num]) {
                continue;
            }

            board[i][j] = (char)(num + '0' + 1);
            rowUsed[i][num] = true;
            colUsed[num][j] = true;
            cubUsed[3 * (i / 3) + j / 3][num] = true;
            if (dfs(rowUsed, colUsed, cubUsed, i, j + 1, board)) {
                return true;
            }
            rowUsed[i][num] = false;
            colUsed[num][j] = false;
            cubUsed[3 * (i / 3) + j / 3][num] = false;
            board[i][j] = '.';
        } else {
            return dfs(rowUsed, colUsed, cubUsed, i, j + 1, board);
        }
    }

    return false;
}
```
