# L398 Longest Increasing Continuous Subsequence II

Give you an integer matrix (with row size n, column size m)，find the longest increasing continuous subsequence in this matrix. (The definition of the longest increasing continuous subsequence here can start at any row or column and go up/down/right/left any direction).

**Example**

Given a matrix:

```
[
  [1 ,2 ,3 ,4 ,5],
  [16,17,24,23,6],
  [15,18,25,22,7],
  [14,19,20,21,8],
  [13,12,11,10,9]
]
```

return`25`

[**Challenge**](http://www.lintcode.com/en/problem/longest-increasing-continuous-subsequence-ii/#challenge)

O(nm) time and memory.

这题因为状态转移顺序难确定，而且初始状态在哪里难找，所以用memorization来解。我们用一个二维的dp矩阵来表示每个格子出发所能达到的最长长度。用一个dfs递归地去填格子，填的时候带上visited数组，如果访问过的直接返回。例如，在找23的时候，我们如果发现24已经找过了，就不用递归地找24（递归进25）。填好格子后，我们跟global max比较一下，如果比现在的大，我们更新global max。至于递归里面，我们用方向矩阵来控制下一个要找的对象。每次发现在下一个比现在这个数字大，而且在边界内，我们就可以进入下一层递归了。当我们递归到底时返回。返回值一开始设为1，因为每一个都可以做一个subsequence。如果我们递归以后发现大了，就更新一下。退出递归时返回这个值。（这个格子的能找到的最长subsequence）。这里写一个强行用递推做的思路，先把矩阵里的数字排序，做成一个数据结构来排，例如，（数字1，在\[i]\[j]位置）然后把最小的开始往dp矩阵放。放的时候，看看上下左右有没有比自己小的数，有就把那个格子的数+1，格子里记录的是，到那个格子为止最长的序列的长度。因为数字都是从小到大放进去的，所以可以这样做。转移方程是dp\[i]\[j] = max(dp四周) + 1。但是呢，搜的顺序要按照从小到大，很难用循环控制，比较麻烦。譬如，这题的顺序是,(0,0)->(0,1)->(0,2)->(0,3)->(0,4)->**(1,4)**...

```java
int[] x = { 0, 0, 1, -1 };
int[] y = { 1, -1, 0, 0 };

public int longestIncreasingContinuousSubsequenceII(int[][] A) {
    if (A == null || A.length == 0 || A[0].length == 0) {
        return 0;
    }

    int[][] dp = new int[A.length][A[0].length];
        int max = 0;
    boolean[][] visited = new boolean[A.length][A[0].length];
    // do dfs for every location, if the location is already set, skip it
    for (int i = 0; i < A.length; i++) {
        for (int j = 0; j < A[0].length; j++) {
            dp[i][j] = dfs(visited, i, j, A, dp);
            max = Math.max(dp[i][j], max);
        }
    }

    return max;
}

private int dfs(boolean[][] visited, int row, int col, int[][] A, int[][] dp) {
    if (visited[row][col]) {
        return dp[row][col];
    }

    int ret = 1;
    for (int k = 0; k < 4; k++) {
        int ni = row + x[k];
        int nj = col + y[k];
        if (inBound(ni, nj, A) && A[ni][nj] > A[row][col]) {
            ret = Math.max(ret, dfs(visited, ni, nj, A, dp) + 1);
        }
    }
        visited[row][col] = true;
        dp[row][col] = ret;
    return ret;
}

private boolean inBound(int i, int j, int[][] A) {
    if (i < 0 || j < 0 || i >= A.length || j >= A[0].length) {
        return false;
    }

    return true;
}
```

可以省visited matrix：

```java
int[] x = {0, 0, 1, -1};
int[] y = {1, -1, 0, 0};

public int longestIncreasingPath(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return 0;
    }

    int n = matrix.length;
    int m = matrix[0].length;

    int[][] dp = new int[n][m];
    int max = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            dp[i][j] = dfs(matrix, i, j, dp);
            max = Math.max(max, dp[i][j]);
        }
    }

    return max;
}

    private int dfs(int[][] matrix, int row, int col, int[][] dp) {
        if (dp[row][col] > 0) {
            return dp[row][col];
        }

        int max = 1;
        for (int k = 0; k < 4; k++) {
            int ni = row + x[k];
            int nj = col + y[k];

            if (inBound(ni, nj, matrix) && matrix[ni][nj] > matrix[row][col]) {
                int ret = dfs(matrix, ni, nj, dp) + 1;
                max = Math.max(ret, max);
            }
        }

        dp[row][col] = max;
        return dp[row][col];
    }

private boolean inBound(int row, int col, int[][] m) {
    if (row < 0 || col < 0 || row >= m.length || col >= m[0].length) {
        return false;
    }

    return true;
}
```
