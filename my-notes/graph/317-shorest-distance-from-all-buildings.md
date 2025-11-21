# 317 Shorest Distance from All Buildings

## 317 Shorest Distance from All Buildings

You want to build a house on anemptyland which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values**0**,**1**or**2**, where:

* Each **0** marks an empty land which you can pass by freely.
* Each **1** marks a building which you cannot pass through.
* Each **2** marks an obstacle which you cannot pass through.

For example, given three buildings at`(0,0)`,`(0,4)`,`(2,2)`, and an obstacle at`(0,2)`:

```
1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```

The point`(1,2)`is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

**Note:**\
There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

## L573 Build Post Office II

Given a 2D grid, each cell is either a wall`2`, an house`1`or empty`0`(the number zero, one, two), find a place to build a post office so that the sum of the distance from the post office to all the houses is smallest.

Return the smallest sum of distance. Return`-1`if it is not possible.

### Notice

* You cannot pass through wall and house, but can pass through empty.
* You only build post office on an empty.

**Example**

Given a grid:

```
0 1 0 0 0
1 0 0 2 1
0 1 0 0 0
```

return`8`, You can build at`(1,1)`. (Placing a post office at (1,1), the distance that post office to all the house sum is smallest.)

[**Challenge**](http://www.lintcode.com/en/problem/build-post-office-ii/#challenge)

Solve this problem within`O(n^3)`time.

这题是逆向bfs，正向的做法是，从每个空地到每个house做一次bfs，这样复杂度会是O(m \* n \* m \* n)。不过因为通常house比空地少，所以我们从house出发对每个空地做一次bfs。这样复杂度就会变成house number × m × n。这里我们要用几个变量辅助。首先要用一个reach矩阵来记录每个空地能被多少个房子visit到，最后找邮局位置时用来判断是否可达。然后用另外一个矩阵dist来记录bfs的距离。当然每次bfs都要带一个visited矩阵防止走回头路。

```java
public int shortestDistance(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return -1;
    }
    // basically use bfs to record distance from 1 (house) to each 0.
    // this question happen to have more 0 than 1, so it's quicker to do bfs on house (1)
    int n = grid.length;
    int m = grid[0].length;
    int[][] dist = new int[n][m];// track distance
    int[][] reach = new int[n][m];// track whether the location is reachable from all houses
    int houseNum = 0; // use at the end to find out which location is reachable in reach[][]

    int[] x = {0, 0, 1, -1};
    int[] y = {1, -1, 0, 0};

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) { // do bfs for houses
                houseNum++;
                boolean[][] visited = new boolean[n][m];
                Queue<Integer> queue = new LinkedList<>();
                int cur = i * m + j;// easier way to bfs matrix, don't have to make a class pair (i, j)

                visited[i][j] = true;
                queue.offer(cur);
                int level = 0;                     
                while (!queue.isEmpty()) {
                    int size = queue.size();
                    for (int s = 0; s < size; s++) {
                        cur = queue.poll();
                        int row = cur / m;
                        int col = cur % m;

                        // bfs on surrounding cells
                        for (int k = 0; k < 4; k++) {
                            int nexti = row + x[k];
                            int nextj = col + y[k];
                            if (inBound(grid, nexti, nextj) && !visited[nexti][nextj] && grid[nexti][nextj] == 0) {
                                visited[nexti][nextj] = true;
                                reach[nexti][nextj]++;
                                dist[nexti][nextj] += level + 1;
                                queue.offer(nexti * m + nextj);
                            }
                        }
                    }

                    level++;
                }
            }
        }
    }

    int min = Integer.MAX_VALUE;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (reach[i][j] == houseNum && grid[i][j] == 0) {
                min = Math.min(min, dist[i][j]);
            }
        }
    }

    return min == Integer.MAX_VALUE ? -1 : min;
}

private boolean inBound(int[][] grid, int r, int c) {
    if (r < 0 || c < 0 || r >= grid.length || c >= grid[0].length) {
        return false;
    }

    return true;
}
```
