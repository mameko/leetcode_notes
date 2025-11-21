# L477 Surrounded Regions

Given a 2D board containing`'X'`and`'O'`, capture all regions surrounded by`'X'`.

A region is captured by flipping all`'O'`'s into`'X'`'s in that surrounded region.

**Example**

```
X X X X
X O O X
X X O X
X O X X
```

After capture all regions surrounded by`'X'`, the board should be:

```
X X X X
X X X X
X X X X
X O X X
```

这题有多种解法，解法一：BFS

```java
public void solve(char[][] board) {

    if (board == null || board.length == 0 || board[0].length == 0) {
        return;
    }

    int n = board.length;
    int m = board[0].length;

    // fill the edge 'O' with 'T'
    for (int i = 0; i < n; i++) {
        if (board[i][0] == 'O') {
            board = bfsfill(board, i, 0, 'O', 'T');
        }

        if (board[i][m - 1] == 'O') {
            board = bfsfill(board, i, m - 1, 'O', 'T');
        }
    }

    for (int j = 0; j < m; j++) {
        if (board[0][j] == 'O') {
            board = bfsfill(board, 0, j, 'O', 'T');
        }

        if (board[n - 1][j] == 'O') {
            board = bfsfill(board, n - 1, j, 'O', 'T');
        }
    }

    // turn rest of 'O' to 'X'
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (board[i][j] == 'O') {
                board = bfsfill(board, i, j, 'O', 'X');
            }
        }
    }

    // turn 'T' back to 'O'
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (board[i][j] == 'T') {
                board = bfsfill(board, i, j, 'T', 'O');
            }
        }
    }
}

int[] x = { 0, 0, 1, -1 };
int[] y = { 1, -1, 0, 0 };

private char[][] bfsfill(char[][] M, int i, int j, char src, char target) {
    Queue<Integer> q = new LinkedList<>();
    int m = M[0].length;
    q.offer(i * m + j);
        M[i][j] = target;

    while (!q.isEmpty()) {
        int cur = q.poll();
        int curi = cur / m;
        int curj = cur % m;

        for (int k = 0; k < 4; k++) {
            int nexti = curi + x[k];
            int nextj = curj + y[k];

            if (inBound(M, nexti, nextj) && M[nexti][nextj] == src) {
                M[nexti][nextj] = target;
                q.offer(nexti * m + nextj);
            }
        }
    }

    return M;
}

private boolean inBound(char[][] M, int row, int col) {
    if (row < 0 || col < 0 || row >= M.length || col >= M[0].length) {
        return false;
    }

    return true;
}
```

方法二：Union Find

```java
public class Solution {
    /**
     * @param board a 2D board containing 'X' and 'O'
     * @return void
     */

    HashMap<Integer, DSNode> map = new HashMap<>(); 
    public void surroundedRegions(char[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return;
        }

        int n = board.length;
        int m = board[0].length;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                makeSet(i * m + j);
            }
        }

        DSNode edgesNodes = null;
        for (int i = 0; i < n; i++) {
            if (board[i][0] == 'O') {
                if (edgesNodes == null) {
                    edgesNodes = unionNei(i, 0, board);
                } else {
                    connect(edgesNodes, unionNei(i, 0, board));
                }
            }

            if (board[i][m - 1] == 'O') {
                if (edgesNodes == null) {
                    edgesNodes = unionNei(i, m - 1, board);
                } else {
                    connect(edgesNodes, unionNei(i, m - 1, board));
                }
            }
        }

        for (int j = 0; j < m; j++) {
            if (board[0][j] == 'O') {
                if (edgesNodes == null) {
                    edgesNodes = unionNei(0, j, board);
                } else {
                    connect(edgesNodes, unionNei(0, j, board));
                }
            }

            if (board[n - 1][j] == 'O') {
                if (edgesNodes == null) {
                    edgesNodes = unionNei(n - 1, j, board);
                } else {
                    connect(edgesNodes, unionNei(n - 1, j, board));
                }
            }
        }

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (board[i][j] == 'O' && (findSet(map.get(i * m + j)) != edgesNodes)) {
                    board[i][j] ='X';
                }
            }
        }

    }

    private DSNode unionNei(int row, int col, char[][] board) {
        int n = board.length;
        int m = board[0].length;

        boolean[][] visited = new boolean[n][m];
        Queue<Integer> q = new LinkedList<>();
        q.offer(row * m + col);
        visited[row][col] = true;

        DSNode res = map.get(row * m + col);

        int[] x = {0, 0, 1, -1};
        int[] y = {1, -1, 0, 0};

        while (!q.isEmpty()) {
            Integer cur = q.poll();
            int curi = cur / m;
            int curj = cur % m;

            for (int k = 0; k < 4; k++) {
                int nexti = curi + x[k];
                int nextj = curj + y[k];

                if (inBound(nexti, nextj, n, m) && board[nexti][nextj] == 'O' && !visited[nexti][nextj]) {
                    visited[nexti][nextj] = true;
                    connect(res, map.get(nexti * m + nextj));
                    q.offer(nexti * m + nextj);
                }
            }
        }

        return res;
    }

    private boolean inBound(int row, int col, int n, int m) {
        if (row < 0 || col < 0 || row >= n || col >= m) {
            return false;
        }

        return true;
    }

    private DSNode connect(DSNode node1, DSNode node2) {
        DSNode p1 = findSet(node1);
        DSNode p2 = findSet(node2);

        if (p1 == p2) {
            return p1;
        }

        if (p1.rank >= p2.rank) {
            p1.rank += p2.rank;
            p2.parent = p1;
            p2.rank = -1;
            return p1;
        } else {
            p2.rank += p1.rank;
            p1.parent = p2;
            p1.rank = -1;
            return p2;
        }
    }

    private DSNode findSet(DSNode node) {
        DSNode p = node.parent;

        if (p == node) {
            return node;
        }

        return p.parent = findSet(p.parent);
    }


    private void makeSet(int node) {
        DSNode newNode = new DSNode(node);
        newNode.rank = 1;
        newNode.parent = newNode;
        map.put(node, newNode);
    }
}

class DSNode {
    int val;
    int rank;
    DSNode parent;

    public DSNode(int v) {
        val = v;
    }
}
```
