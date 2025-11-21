# 529 Minesweeper

Let's play the minesweeper game ([Wikipedia](https://en.wikipedia.org/wiki/Minesweeper_\(video_game\)),[online game](http://minesweeperonline.com/))!

You are given a 2D char matrix representing the game board.**'M'**&#x72;epresents an**unrevealed**mine,**'E'**&#x72;epresents an**unrevealed**empty square,**'B'**&#x72;epresents a**revealed**blank square that has no adjacent (above, below, left, right, and all 4 diagonals) mines,**digit**('1' to '8') represents how many mines are adjacent to this**revealed**square, and finall&#x79;**'X'**&#x72;epresents a**revealed**mine.

Now given the next click position (row and column indices) among all the**unrevealed**squares ('M' or 'E'), return the board after revealing this position according to the following rules:

1.  If a mine ('M') is revealed, then the game is over - change it to

    **'X'**

    .
2.  If an empty square ('E') with

    **no adjacent mines**

    is revealed, then change it to revealed blank ('B') and all of its adjacent

    **unrevealed**

    squares should be revealed recursively.
3.  If an empty square ('E') with

    **at least one adjacent mine**

    is revealed, then change it to a digit ('1' to '8') representing the number of adjacent mines.
4. Return the board when no more squares will be revealed.

**Example 1:**

```
Input: 

[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]

Click : [3,0]


Output: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

**Example 2:**

```
Input: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Click : [1,2]


Output: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
```

**Note:**

1. The range of the input matrix's height and width is \[1,50].
2. The click position will only be an unrevealed square ('M' or 'E'), which also means the input board contains at least one clickable square.
3. The input board won't be a stage when game is over (some mines have been revealed).
4. For simplicity, not mentioned rules should be ignored in this problem. For example, you **don't** need to reveal all the unrevealed mines when the game is over, consider any cases that you will win the game or flag any squares.

这题BFS是一看就知道的了，不过呢，一开始没搞懂进队顺序，所以用了一个非常复杂的方法做，然后看了discuss才发现，数完雷再入队的话就可以防止把不该进队的进了。

discuss的方法：

```java
public char[][] updateBoard(char[][] board, int[] click) {
    if (board == null || board.length == 0 || board[0].length == 0) {
        return board;
    }

    int n = board.length;
    int m = board[0].length;

    int clickr = click[0];
    int clickc = click[1];

    // if click on 'M', turn the cell into 'X' and return
    if (board[clickr][clickc] == 'M') {
        board[clickr][clickc] = 'X';
        return board;
    }

    // if click on already opened cell return
    if (board[clickr][clickc] != 'E') {
        return board;
    }

    // if click on blank

    int[] x = { 0, 0, 1, -1, 1, -1, -1, 1 };
    int[] y = { 1, -1, 0, 0, 1, 1, -1, -1 };


    Queue<Integer> q = new LinkedList<>();
    q.offer(clickr * m + clickc);

    while (!q.isEmpty()) {
        Integer cur = q.poll();
        Integer curi = cur / m;
        Integer curj = cur % m;

            int cnt = 0;
        for (int k = 0; k < 8; k++) {
            Integer ni = curi + x[k];
            Integer nj = curj + y[k];

            if (inBound(ni, nj, board) && board[ni][nj] == 'M') {
                    cnt++;
            }
        }

            if (cnt > 0) {
            board[curi][curj] = (char)(cnt + '0');
            } else {
                board[curi][curj] = 'B';
                for (int k = 0; k < 8; k++) {
                    Integer ni = curi + x[k];
                    Integer nj = curj + y[k];

                    if (inBound(ni, nj, board) && board[ni][nj] == 'E') {
                        q.offer(ni * m + nj);
                        board[ni][nj] = 'B';
                    }
                }
            }
    }

    return board;
}

private boolean inBound(int row, int col, char[][] b) {
    if (row < 0 || col < 0 || row >= b.length || col >= b[0].length) {
        return false;
    }

    return true;
}
```

我的方法:

```java
public char[][] updateBoard(char[][] board, int[] click) {
    if (board == null || board.length == 0 || board[0].length == 0) {
        return board;
    }

    int n = board.length;
    int m = board[0].length;

    int clickr = click[0];
    int clickc = click[1];

    // if click on 'M', turn the cell into 'X' and return
    if (board[clickr][clickc] == 'M') {
        board[clickr][clickc] = 'X';
        return board;
    }

    // if click on already opened cell return
    if (board[clickr][clickc] != 'E') {
        return board;
    }

    // if click on blank

    int[] x = { 0, 0, 1, -1, 1, -1, -1, 1 };
    int[] y = { 1, -1, 0, 0, 1, 1, -1, -1 };

    // 0. need to record which place is not opened 
    boolean[][] notOpened = new boolean[n][m];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (board[i][j] == 'E') {
                notOpened[i][j] = true;
            }
        }
    }

    // 1. add up mines around each mine first
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (board[i][j] == 'M') {
                for (int k = 0; k < 8; k++) {
                    int ni = i + x[k];
                    int nj = j + y[k];
                    if (inBound(ni, nj, board)) {
                        if (board[ni][nj] == 'E') {
                            board[ni][nj] = '1';
                        } else if (Character.isDigit(board[ni][nj]) && notOpened[ni][nj]) {
                            board[ni][nj]++;
                        }
                    }
                }
            }
        }
    }

    // 2. bfs to turn 'E' into 'B'
    Queue<Integer> q = new LinkedList<>();
    if (board[clickr][clickc] == 'E') {
        q.offer(clickr * m + clickc);
    }
    notOpened[clickr][clickc] = false;

    while (!q.isEmpty()) {
        Integer cur = q.poll();
        Integer curi = cur / m;
        Integer curj = cur % m;

        board[curi][curj] = 'B';

        for (int k = 0; k < 8; k++) {
            Integer ni = curi + x[k];
            Integer nj = curj + y[k];

            if (inBound(ni, nj, board) && notOpened[ni][nj] && board[ni][nj] != 'M') {
                if (board[ni][nj] == 'E') {
                    q.offer(ni * m + nj);
                }
                notOpened[ni][nj] = false;
            }
        }
    }

    // 3. if the cell is not visited, turn it back to 'E'
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (notOpened[i][j] && Character.isDigit(board[i][j])) {
                board[i][j] = 'E';
            }
        }
    }

    return board;
}

private boolean inBound(int row, int col, char[][] b) {
    if (row < 0 || col < 0 || row >= b.length || col >= b[0].length) {
        return false;
    }

    return true;
}
```
