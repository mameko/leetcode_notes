# 419 Battleships in a Board

Given an 2D board, count how many battleships are in it. The battleships are represented with`'X'`s, empty slots are represented with`'.'`s. You may assume the following rules:

* You receive a valid board, made of only battleships or empty slots.
* Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape`1xN`(1 row, N columns) or`Nx1`(N rows, 1 column), where N can be of any size.
* At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.

**Example:**

```
X..X
...X
...X
```

In the above board there are 2 battleships.

**Invalid Example:**

```
...X
XXXX
...X
```

This is an invalid board that you will not receive - as battleships will always have a cell separating between them.

**Follow up:**\
Could you do it in**one-pass**, using only**O(1) extra memory**and**without modifying**the value of the board?

这题一眼看上去，不就是[**number of island**](200-number-of-islands.md)吗？所以第一种做法用了extra memory。T:O(n^2), S:O(n^2)。做法二就是follow up的答案，因为board一定是valid的，所以我们可以从左上到右下直接数。把不是船的和已经数过的跳过，剩下的就++。

做法二：

```java
public int countBattleships(char[][] board) {
    if (board == null || board.length == 0 || board[0].length == 0) {
        return 0;
    }

    int n = board.length;
    int m = board[0].length;
    int res = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (board[i][j] == '.' || // not part of a ship
                i > 0 && board[i - 1][j] == 'X' || // part of previously counted ship, vertical
                j > 0 && board[i][j - 1] == 'X') { // part of previously counted ship, horizontal
                continue;
            }
            res++;
        }
    }

    return res;
}
```

做法一：

```java
public int countBattleships(char[][] board) {
    if (board == null || board.length == 0 || board[0].length == 0) {
        return 0;
    }

    int n = board.length;
    int m = board[0].length;
    int res = 0;
    boolean[][] visited = new boolean[n][m];

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && board[i][j] == 'X') {
                res++;
                visited[i][j] = true;
                markShip(visited, board, i, j);
            }
        }
    }

    return res;
}

private void markShip(boolean[][] visited, char[][] board, int row, int col) {
    while (col + 1 < board[0].length && !visited[row][col + 1] && board[row][col + 1] == 'X') {
            visited[row][col + 1] = true;
            col++;
        }

        while (row + 1 < board.length && !visited[row + 1][col] && board[row + 1][col] == 'X') {
            visited[row + 1][col] = true;
            row++;
        }
}
```
