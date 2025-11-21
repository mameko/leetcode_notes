# 542 01 Matrix

Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1.

**Example 1:**

```
Input:
[[0,0,0],
 [0,1,0],
 [0,0,0]]

Output:
[[0,0,0],
 [0,1,0],
 [0,0,0]]
```

**Example 2:**

```
Input:
[[0,0,0],
 [0,1,0],
 [1,1,1]]

Output:
[[0,0,0],
 [0,1,0],
 [1,2,1]]
```

**Note:**

1. The number of elements of the given matrix will not exceed 10,000.
2. There are at least one 0 in the given matrix.
3. The cells are adjacent in only four directions: up, down, left and right.

2023再刷，原来的bfs过不了大数据了。然后看了答案，发现有另一种优化一点的bfs思路。像zombie那题那样，一开始把所有0入队，和把所有1的位置设成max。每次，我们拿一个出来，看看邻居有没有要更新的。如果邻居是0，就不用更新。如果是1，一开始会被更新成1，0 + 1 < max。然后，把被更新了的1入队。每一次，找到了可以更新的地方，我们就比较一下现在这个值+1是否小于next已有的值。如果小于，就意味着这边有更近的0.因为我们是从所有0出发，所以找到的更新的位置就是最靠近某个0的位置。这个解法比较给力，只用了T: O(r \* c)和S:O(r \* c)，queue的size，因为只做了一次bfs

solultion里还有一种dp的解法，感觉有点像股票或者降雨题那些，正着来一遍，反着来一遍。这里因为每个1的格子都是来自上下左右四格+1取min。先左到右下上做一遍，再右下到左上做一遍。这样就能上下左右的都min了。T: O(r \* c)，听说这里没用额外空间。

```java
public int[][] updateMatrix(int[][] mat) {
    if (mat == null || mat.length == 0 || mat[0].length == 0) {
        return mat;
    }

    int[] x = {0, 0, 1, -1};
    int[] y = {1, -1, 0, 0};

    int rowLen = mat.length;
    int colLen = mat[0].length;

    int[][] dist = new int[rowLen][colLen];
    Queue<int[]> queue = new LinkedList<>();
    for (int i = 0; i < rowLen; i++) {
        for (int j = 0; j < colLen; j++) {                
            if (mat[i][j] == 1) {
                dist[i][j] = Integer.MAX_VALUE;
                continue;
            }

            queue.offer(new int[] { i, j });
        }
    }
    
    while (!queue.isEmpty()) {
        int[] cur = queue.poll();
            
        for (int k = 0; k < 4; k++) {
            int nexti = cur[0] + x[k];
            int nextj = cur[1] + y[k];

            if (!inbound(nexti, nextj, rowLen, colLen)) {
                continue;
            }

            if (dist[nexti][nextj] > dist[cur[0]][cur[1]] + 1) {                        
                dist[nexti][nextj] = dist[cur[0]][cur[1]] + 1;
                queue.offer(new int[] { nexti, nextj });
            }
        }
    }
    return dist;
}

private boolean inbound(int i, int j, int row, int col) {
    if (i < 0 || j < 0 || i >= row || j >= col) {
        return false;
    }
    return true;
}
```



经典题，就是把grid看成图，上下左右bfs。有1的地方算一算，填答案。还是一如既往返回距离时要小心，step得想清楚。k个1的话，就T：O(kn^2)，最坏情况全图是1所以n^2，S:O(n^2)

```java
public int[][] updateMatrix(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return matrix; 
    }

    int n = matrix.length;
    int m = matrix[0].length;

    int[][] res = new int[n][m];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] == 1)  {
                res[i][j] = bfs(matrix, i, j);
            }
        }
    }

    return res;
}

int[] x = {1, -1, 0, 0};
int[] y = {0, 0, 1, -1};

private int bfs(int[][] matrix, int i, int j) {
    int n = matrix.length;
    int m = matrix[0].length;
    boolean[][] visited = new boolean[n][m];

    Queue<Integer> queue = new LinkedList<>();
    queue.offer(i * m + j);
    visited[i][j] = true;
    int step = 1;        
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int s = 0; s < size; s++) {
            int cur = queue.poll();
            int curi = cur / m;
            int curj = cur % m;

            for (int k = 0; k < 4; k++) {
                int nexti = curi + x[k];
                int nextj = curj + y[k];
                if (inBound(nexti, nextj, m, n) && matrix[nexti][nextj] == 0) {
                    return step;
                }
                if (inBound(nexti, nextj, m, n) && !visited[nexti][nextj] &&  matrix[nexti][nextj] == 1) {
                    queue.offer(nexti * m + nextj);
                }
            }
        }
        step++;
    }
    return step;
}

private boolean inBound(int i, int j, int m, int n) {
    if (i < 0 || j < 0 || i >= n || j >= m) {
        return false;
    } else {
        return true;
    }

}
```
