# 79 Word Search

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,\
Given **board**=

```
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```

**word** =`"ABCCED"`, -> returns`true`,

**word** =`"SEE"`, -> returns`true`,

**word** =`"ABCB"`, -> returns`false`.

这里要注意backtrack，然后递归结束条件：如果传入start = 1的话，结束条件是== word len， 如果传入是0的话，结束条件是== word len - 1. T: O(n^2 \* 4^len), 首先要n^2 loop着来找起点，每次dfs找4个方向，然后要找len次。S：O(n),dfs深度

```java
public boolean exist(char[][] board, String word) {
    if (board == null || board.length == 0 || board[0].length == 0 || word == null || word.isEmpty()) {
        return false;
    }

    int n = board.length;
    int m = board[0].length;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (board[i][j] == word.charAt(0)) {
                boolean[][] visited = new boolean[n][m];
                visited[i][j] = true;
                if (dfs(board, word, visited, i, j, 1)) {
                    return true;
                }
            }
        }
    }

    return false;
}

private boolean dfs(char[][] board, String word, boolean[][] visited, int row, int col, int start) {
    if (start == word.length()) {
        return true;
    }

    int[] x = {0, 0, 1, -1};
    int[] y = {1, -1, 0, 0};

    for (int k = 0; k < 4; k++) {
        int ni = row + x[k];
        int nj = col + y[k];

        if (inBound(ni, nj, board) && !visited[ni][nj] && word.charAt(start) == board[ni][nj]) {
            visited[ni][nj] = true;
            if (dfs(board, word, visited, ni, nj, start + 1)) {
                return true;
            }
            visited[ni][nj] = false;
        }
    }

    return false;
}

private boolean inBound(int row, int col, char[][] board) {
    if (row < 0 || col < 0 || row >= board.length || col >= board[0].length) {
        return false;
    }
    return true;
}
```

下面这个版本的回溯有一点点不同，写下来作为参考

```java
    int[] x = {0, 0, 1, -1}; 
    int[] y = {1, -1, 0, 0}; 
    public boolean exist(char[][] board, String word) {
        if (word == null || word.length() == 0 || board == null || board.length == 0 || board[0].length == 0) {
            return false;
        }

        char[] cs = word.toCharArray();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == cs[0]) {
                    boolean[][] visited = new boolean[board.length][board[0].length];
                    visited[i][j] = true;
                    boolean exist = dfs(board, cs, i, j, visited, 0);

                    if (exist) {
                        return true;
                    }
                }
            }
        }

        return false;
    }

    private boolean dfs(char[][] board, char[] cs, int row, int col, boolean[][] visited, int start) {
        if (start == cs.length - 1) {
            return true;
        }

        for (int i = 0; i < 4; i++) {
            int nexti = row + x[i];
            int nextj = col + y[i];

            if (inBound(nexti, nextj, board) && !visited[nexti][nextj] && (board[nexti][nextj] == cs[start + 1])) {
                visited[nexti][nextj] = true;
                boolean res = dfs(board, cs, nexti, nextj, visited, start + 1);
                visited[nexti][nextj] = false;

                if (res) {
                    return true;
                }
            }
        }

        return false;
    }

    private boolean inBound(int row, int col, char[][] board) {
        if (row >= board.length || row < 0 || col < 0 || col >= board[0].length) {
            return false;
        }

        return true;
    }
```
