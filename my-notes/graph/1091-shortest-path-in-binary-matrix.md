# 1091 Shortest Path in Binary Matrix

Given an `n x n` binary matrix `grid`, return _the length of the shortest **clear path** in the matrix_. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:

* All the visited cells of the path are `0`.
* All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

```
Input: grid = [[0,1],[1,0]]
Output: 2
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

```
Input: grid = [[0,0,0],[1,1,0],[1,1,0]]
Output: 4
```

**Example 3:**

```
Input: grid = [[1,0,0],[1,1,0],[1,1,0]]
Output: -1
```

**Constraints:**

* `n == grid.length`
* `n == grid[i].length`
* `1 <= n <= 100`
* `grid[i][j] is 0 or 1`

这题一看，不就是bfs吗？然后快速地写了。T:O(n), S:O(n)。看了答案发现还有A\*的做法。复杂度不变，而且实现复杂容易错，建议先上bfs，做好了以后，再提提A\*。这题还有一种bfs是直接改原矩阵，把距离填到上面已经visit过的node。因为我们每次找neighbor的时候，只会找那些‘0’的，所以可以这样处理。但这个做法有个缺陷，如果是多线程的就不行了。

```java
public int shortestPathBinaryMatrix(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return -1;
    }

    int n = grid.length;

    if (grid[0][0] != 0 || grid[n - 1][n - 1] != 0) {
        return -1;
    }

    boolean[][] visited = new boolean[n][n];
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(0);
    visited[0][0] = true;

    int[] x = {0, 0, -1, 1, 1, -1, -1, 1};
    int[] y = {-1, 1, 0, 0, 1, -1, 1, -1};

    int level = 1;
    int result = -1;
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int cur = queue.poll();
            int curi = cur / n;
            int curj = cur % n;

            if (curi == n - 1 && curj == n - 1) {
                return level;
            }
            for (int k = 0; k < 8; k++) {
                int nexti = curi + x[k];
                int nextj = curj + y[k];

                if (inBound(nexti, nextj, n) && !visited[nexti][nextj] && grid[nexti][nextj] == 0) {
                    queue.offer(nexti * n + nextj);
                    visited[nexti][nextj] = true;
                }
            }
        }
        level++;
    }
    return result;
}

private boolean inBound(int i, int j, int n) {
    if (i < 0 || j < 0 || i >= n || j >= n) {
        return false;
    } else {
        return true;
    }
}

// 贴一个A*，T:O(nlogn)，因为heap。S:O(n)
class Solution {
    
    // Candidate represents a possible option for travelling to the cell
    // at (row, col).
    class Candidate {
        
        public int row;
        public int col;
        public int distanceSoFar;
        public int totalEstimate;
        
        public Candidate(int row, int col, int distanceSoFar, int totalEstimate) {
            this.row = row;
            this.col = col;
            this.distanceSoFar = distanceSoFar;
            this.totalEstimate = totalEstimate;
        }
    }
    
    
    private static final int[][] directions = 
        new int[][]{{-1, -1}, {-1, 0}, {-1, 1}, {0, -1}, {0, 1}, {1, -1}, {1, 0}, {1, 1}};
    
    
    public int shortestPathBinaryMatrix(int[][] grid) {
        
        // Firstly, we need to check that the start and target cells are open.
        if (grid[0][0] != 0 || grid[grid.length - 1][grid[0].length - 1] != 0) {
            return -1;
        }
        
        // Set up the A* search.
        Queue<Candidate> pq = new PriorityQueue<>((a,b) -> a.totalEstimate - b.totalEstimate);
        pq.add(new Candidate(0, 0, 1, estimate(0, 0, grid)));
        
        boolean[][] visited = new boolean[grid.length][grid[0].length];
        
        // Carry out the A* search.
        while (!pq.isEmpty()) {
            
            Candidate best = pq.remove();
            
            // Is this for the target cell?
            if (best.row == grid.length - 1 && best.col == grid[0].length - 1) {
                return best.distanceSoFar;
            }
            
            // Have we already found the best path to this cell?
            if (visited[best.row][best.col]) {
                continue;
            }
            
            visited[best.row][best.col] = true;
            
            for (int[] neighbour : getNeighbours(best.row, best.col, grid)) {
                int neighbourRow = neighbour[0];
                int neighbourCol = neighbour[1];
                
                // This check isn't necessary for correctness, but it greatly
                // improves performance.
                if (visited[neighbourRow][neighbourCol]) {
                    continue;
                }
                
                // Otherwise, we need to create a Candidate object for the option
                // of going to neighbor through the current cell.
                int newDistance = best.distanceSoFar + 1;
                int totalEstimate = newDistance + estimate(neighbourRow, neighbourCol, grid);
                Candidate candidate = 
                    new Candidate(neighbourRow, neighbourCol, newDistance, totalEstimate);
                pq.add(candidate);
            }   
        }
        // The target was unreachable.
        return -1;  
    }
    
    private List<int[]> getNeighbours(int row, int col, int[][] grid) {
        List<int[]> neighbours = new ArrayList<>();
        for (int i = 0; i < directions.length; i++) {
            int newRow = row + directions[i][0];
            int newCol = col + directions[i][1];
            if (newRow < 0 || newCol < 0 || newRow >= grid.length 
                    || newCol >= grid[0].length
                    || grid[newRow][newCol] != 0) {
                continue;    
            }
            neighbours.add(new int[]{newRow, newCol});
        }
        return neighbours; 
    }
    
    // Get the best case estimate of how much further it is to the bottom-right cell.
    private int estimate(int row, int col, int[][] grid) {
        int remainingRows = grid.length - row - 1;
        int remainingCols = grid[0].length - col - 1;
        return Math.max(remainingRows, remainingCols);
    }
}
```
