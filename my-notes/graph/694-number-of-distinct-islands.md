# 694 Number of Distinct Islands

Given a non-empty 2D array`grid`of 0's and 1's, an **island** is a group of`1`'s (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Count the number of **distinct** islands. An island is considered to be the same as another if and only if one island can be translated (and not rotated or reflected) to equal the other.

**Example 1:**

```
11000
11000
00011
00011
```

Given the above grid map, return`1`.

**Example 2:**

```
11011
10000
00001
11011
```

Given the above grid map, return`3`.

Notice that:

```
11
1
```

and

```
 1
11
```

are considered different island shapes, because we do not consider reflection / rotation.

**Note:**&#x54;he length of each dimension in the given`grid`does not exceed 50.

这题感觉基本套路一样，但难点在于怎么判断重复的island。因为搜的顺序是一样的，所以无论是dfs或bfs，pattern都会是一样的。

解法二：其实这个pattern，我们每次都得loop一遍所有找到得island来判断，这样比较低效。所以如果能生成一个unique的key的话，我们每次就只要去hashmap里看看，最后返回map的size就好了。那么怎么生成unique的key呢，其实，因为bfs的顺序是一样的从左上到右下，所以所有同款的island所包含的node加入到这个island的顺序是一样的。我们就用set of set来装。T:O(n\*m), S:O(n \* m)

```java
class Solution {
    int n = 0;
    int m = 0;
    
    public int numDistinctIslands(int[][] grid) {
        if (grid == null || grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        
        n = grid.length;
        m = grid[0].length;
        boolean[][] visited = new boolean[n][m];
        Set<Set<Pair<Integer, Integer>>> islands = new HashSet<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 0 || visited[i][j]) {
                    continue;
                }
                Set<Pair<Integer, Integer>> nodesOfAIsland = new HashSet<>();
                findIsland(i, j, grid, visited, nodesOfAIsland);
                islands.add(nodesOfAIsland);
            }
        }
        
        return islands.size();
    }
    
    int[] x = {0, 0, 1, -1};
    int[] y = {1, -1, 0, 0};    
    private void findIsland(int i, int j, int[][] grid, boolean[][] visited, Set<Pair<Integer, Integer>> nodesOfAIsland) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(i * m + j);
        visited[i][j] = true;
        
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            int curi = cur / m;
            int curj = cur % m;
            nodesOfAIsland.add(new Pair<>(curi - i, curj - j));
            
            for (int k = 0; k < 4; k++) {
                int nexti = curi + x[k];
                int nextj = curj + y[k];
                
                if (inBound(nexti, nextj) && !visited[nexti][nextj] && grid[nexti][nextj] == 1) {
                    queue.offer(nexti * m + nextj);
                    visited[nexti][nextj] = true;                    
                }
            }
        }
    }
    
    private boolean inBound(int i, int j) {
        if (i < 0 || j < 0 || i >= n || j >= m) {
            return false;
        }
        return true;
    }
}
```

解法一：暴力解。一开始想的时候，先用island的size来filter，因为不同size的就不可能是相同pattern的。然后想漏了同样size也会有不同pattern。所以大数据没过。然后判断时候要分两个函数写，一开始合在一起写，写了半天没对。函数还是小点比较好控制。T:O(m^2\*n^2),S:O(n\*m)

```java
public int numDistinctIslands(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return 0;
    }

    int n = grid.length;
    int m = grid[0].length;

    // <cnt, List<pattern>>
    Map<Integer, List<List<Integer>>> map = new HashMap<>();
    int unique = 0;
    boolean[][] visited = new boolean[n][m];

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1 && !visited[i][j]) {
                List<Integer> tmp = new ArrayList<>();
                int cnt = bfs(grid, tmp, visited, i, j);
                // 找到一个island就判断一下是否unique，是的话就++
                if (isUnique(cnt, tmp, map, m)) {
                    unique++;
                }
            }
        }
    }

    return unique;
}

int[] x = {0, 0, 1, -1};
int[] y = {1, -1, 0, 0};

// 经典bfs，边数island size边把island的pattern记到tmp里
private int bfs(int[][] grid, List<Integer> tmp, boolean[][] visited, int i, int j) {
    int n = grid.length;
    int m = grid[0].length;

    Queue<Integer> queue = new LinkedList<>();
    queue.offer(i * m + j);
    visited[i][j] = true;

    int cnt = 0;

    while (!queue.isEmpty()) {
        int cur = queue.poll();
        int curi = cur / m;
        int curj = cur % m;
        cnt++;
        tmp.add(cur);

        for (int k = 0; k < 4; k++) {
            int ni = curi + x[k];
            int nj = curj + y[k];
            if (ni >= 0 && ni < n && nj >= 0 && nj < m && !visited[ni][nj] && grid[ni][nj] == 1) {
                queue.offer(ni * m + nj);
                visited[ni][nj] = true;
            }
        }
    }

    return cnt;
}

// 判断同样size的island之间pattern有没有match
private boolean isUnique(int cnt, List<Integer> tmp, Map<Integer, List<List<Integer>>> map, int m) {
    // 不同size的island是unique的
    if (!map.containsKey(cnt)) {
        map.put(cnt, new ArrayList<>());
        map.get(cnt).add(new ArrayList<>(tmp));
        return true;
    }

    List<List<Integer>> patterns = map.get(cnt);        
    for (List<Integer> pattern : patterns) {            
        if (isMatch(tmp, pattern, m)) {
            return false;// 只要有一个match就证明不是unique的了
        }        
    }
    // 发现unique的记得加到map里
    map.get(cnt).add(new ArrayList<>(tmp));
    return true;
}

//判断两个pattern是否match
private boolean isMatch(List<Integer> tmp, List<Integer> pattern, int m) {        
    int diffi = pattern.get(0) / m - tmp.get(0) / m;
    int diffj = pattern.get(0) % m - tmp.get(0) % m;

    for (int i = 1; i < pattern.size(); i++) {
        int curi = pattern.get(i) / m - tmp.get(i) / m;
        int curj = pattern.get(i) % m - tmp.get(i) % m;
        if (curi != diffi || curj != diffj) {
            return false;
        }
    }    
    return true;
}
```
