# 1380 Lucky Numbers in a Matrix



Given an `m x n` matrix of **distinct** numbers, return _all **lucky numbers** in the matrix in **any** order_.

A **lucky number** is an element of the matrix such that it is the minimum element in its row and maximum in its column.

&#x20;

**Example 1:**

<pre><code>Input: matrix = [[3,7,8],[9,11,13],[15,16,17]]
<strong>Output: [15]
</strong><strong>Explanation: 15 is the only lucky number since it is the minimum in its row and the maximum in its column.
</strong></code></pre>

**Example 2:**

<pre><code>Input: matrix = [[1,10,4,2],[9,3,8,7],[15,16,17,12]]
<strong>Output: [12]
</strong><strong>Explanation: 12 is the only lucky number since it is the minimum in its row and the maximum in its column.
</strong></code></pre>

**Example 3:**

<pre><code>Input: matrix = [[7,8],[1,2]]
<strong>Output: [7]
</strong><strong>Explanation: 7 is the only lucky number since it is the minimum in its row and the maximum in its column.
</strong></code></pre>

&#x20;

**Constraints:**

* `m == mat.length`
* `n == mat[i].length`
* `1 <= n, m <= 50`
* `1 <= matrix[i][j] <= 105`.
* All elements in the matrix are distinct.

这个题还是简单题，就是按描述的内容implement。算法没有特别难的地方，主要是要一次写对也不容易。主要是竖着traver matrix不常写。后面还补了两个discussion里优化点的写法。



```java
public List<Integer> luckyNumbers (int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return result;
    }
    
    int n = matrix.length;
    int m = matrix[0].length;
    int[] minInRow = new int[n];
    int[] maxInCol = new int[m];
    
    for (int i = 0; i < n; i++) {            
        int curMinInRow = Integer.MAX_VALUE;
        for (int j = 0; j < m; j++) {
            if (matrix[i][j] < curMinInRow) {
                curMinInRow = matrix[i][j];
                minInRow[i] = curMinInRow;
            }
        }
    }
    
    for (int i = 0; i < m; i++) {
        int curmaxInCol = Integer.MIN_VALUE;
        for (int j = 0; j < n; j++) {
            if (matrix[j][i] > curmaxInCol) {
                curmaxInCol = matrix[j][i];
                maxInCol[i] = curmaxInCol;
            }
        }
    }
    
    Set<Integer> set = new HashSet<>();
    for (int minNum : minInRow) {
        set.add(minNum);
    }
    
    for (int maxNum : maxInCol) {
        if (set.contains(maxNum)) {
            result.add(maxNum);
        }
    }

    return result;
}

// discussion里的，其实前两个loop可以collapse成一个
public List<Integer> luckyNumbers (int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[] mi = new int[m], mx = new int[n];
    Arrays.fill(mi, Integer.MAX_VALUE);
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            mi[i] = Math.min(matrix[i][j], mi[i]);
            mx[j] = Math.max(matrix[i][j], mx[j]);
        }
    }
    List<Integer> res = new ArrayList<>();
    for (int i = 0; i < m; ++i) {
        for (int j = 0; j < n; ++j) {
            if (mi[i] == mx[j])  {
                res.add(mi[i]);
                break;           // credit to @Ausho_Roup
            }
        }
    }
    return res;        
}

// 这个用了set来做
public List<Integer> luckyNumbers (int[][] matrix) {
    Set<Integer> minSet = new HashSet<>(), maxSet = new HashSet<>();
    for (int[] row : matrix) {
        int mi = row[0];
        for (int cell : row)
            mi = Math.min(mi, cell);
        minSet.add(mi);
    }
    for (int j = 0; j < matrix[0].length; ++j) {
        int mx = matrix[0][j];
        for (int i = 0; i < matrix.length; ++i)
            mx = Math.max(matrix[i][j], mx);
        if (minSet.contains(mx))
            maxSet.add(mx);
    }
    return new ArrayList<>(maxSet);        
}
}j
```
