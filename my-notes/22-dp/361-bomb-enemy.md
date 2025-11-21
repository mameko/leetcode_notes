# 361 Bomb Enemy

Given a 2D grid, each cell is either a wall`'W'`, an enemy`'E'`or empty`'0'`(the number zero), return the maximum enemies you can kill using one bomb.\
The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.

## Notice

You can only put the bomb at an empty cell.

**Example**

Given a grid:

```
0 E 0 0
E 0 W E
0 E 0 0
```

return`3`. (Placing a bomb at (1,1) kills 3 enemies)

这题感觉不是特别dp，就是把4个方向上的敌人都算好了，最后再加起来。两种方法都是O(mn)

```java
public int maxKilledEnemies(char[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int n = grid.length;
    int m = grid[0].length;
    // preprocss 4 matrix for enemy you can boom
    int[][] leftRight = new int[n][m];
    int[][] rightLeft = new int[n][m];
    int[][] topBottom = new int[n][m];
    int[][] bottomTop = new int[n][m];

    // fill left -> right
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if ((j == 0 && grid[i][j] != 'E') || grid[i][j] == 'W') {
                leftRight[i][j] = 0;
            } else if (grid[i][j] == 'E') {
                if (j == 0) {
                    leftRight[i][j] = 1;
                } else {
                    leftRight[i][j] = leftRight[i][j - 1] + 1;
                }
            } else {
                leftRight[i][j] = leftRight[i][j - 1];
            }
        }
    }

    // fill right -> left
    for (int i = 0; i < n; i++) {
        for (int j = m - 1; j >= 0; j--) {
            if ((j == m - 1 && grid[i][j] != 'E') || grid[i][j] == 'W') {
                rightLeft[i][j] = 0;
            } else if (grid[i][j] == 'E') {
                if (j == m - 1) {
                    rightLeft[i][j] = 1;
                } else {
                    rightLeft[i][j] = rightLeft[i][j + 1] + 1;
                }
            } else {
                rightLeft[i][j] = rightLeft[i][j + 1];
            }
        }
    }

    // fill top -> bottom
    for (int j = 0; j < m; j++) {
        for (int i = 0; i < n; i++) {
            if ((i == 0 && grid[i][j] != 'E') || grid[i][j] == 'W') {
                topBottom[i][j] = 0;
            } else if (grid[i][j] == 'E') {
                if (i == 0) {
                    topBottom[i][j] = 1;
                } else {
                    topBottom[i][j] = topBottom[i - 1][j] + 1;
                }
            } else {
                topBottom[i][j] = topBottom[i - 1][j];
            }
        }
    }

    // fill bottom -> top
    for (int j = 0; j < m; j++) {
        for (int i = n - 1; i >= 0; i--) {
            if ((i == n - 1 && grid[i][j] != 'E') || grid[i][j] == 'W') {
                bottomTop[i][j] = 0;
            } else if (grid[i][j] == 'E') {
                if (i == n - 1) {
                    bottomTop[i][j] = 1;
                } else {
                    bottomTop[i][j] = bottomTop[i + 1][j] + 1;
                }
            } else {
                bottomTop[i][j] = bottomTop[i + 1][j];
            }
        }
    }

    int max = 0;
    // add all directions, return max
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == '0') {
                int sum = leftRight[i][j] + rightLeft[i][j] + topBottom[i][j] + bottomTop[i][j];
                max = Math.max(max, sum);
            }
        }
    }

    return max;
}
```

第二个做法不太懂，好像是统计从W到W之间的E个数，行和列分别统计

```java
public int maxKilledEnemies(char[][] grid) {
    int m = grid.length;
    int n = m > 0 ? grid[0].length : 0;
    
    int result = 0;
    int rows = 0;
    int[] cols = new int[n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (j == 0 || grid[i][j - 1] == 'W') {
                row = 0;
                for (int k = j; k < n && grid[i][k] != 'W'; k++) {
                    if (grid[i][k] == 'E') {
                        rows += 1;
                    }
                }
             }
             if (i = 0 || grid[i - 1][j] == 'W') {
                 cols[j] = 0;
                 for (int k = i; k < m && grid[k][j] != 'W'; k++) {
                     if (grid[k][j] == 'E') {
                         cols[j] +=1;
                     }
                 }
             }
             if (grid[i][j] == '0' && rows + cols[j] > result) {
                 result = rows + cols[j];
             }
         }
     
     return result;
 }
```
