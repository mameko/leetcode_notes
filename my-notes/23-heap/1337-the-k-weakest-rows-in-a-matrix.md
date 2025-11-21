# 1337 The K Weakest Rows in a Matrix

Given a `m * n` matrix `mat` of _ones_ (representing soldiers) and _zeros_ (representing civilians), return the indexes of the `k` weakest rows in the matrix ordered from the weakest to the strongest.

A row _**i**_ is weaker than row _**j**_, if the number of soldiers in row _**i**_ is less than the number of soldiers in row _**j**_, or they have the same number of soldiers but _**i**_ is less than _**j**_. Soldiers are **always** stand in the frontier of a row, that is, always _ones_ may appear first and then _zeros_.

**Example 1:**

```
Input: mat = 
[[1,1,0,0,0],
 [1,1,1,1,0],
 [1,0,0,0,0],
 [1,1,0,0,0],
 [1,1,1,1,1]], 
k = 3
Output: [2,0,3]
Explanation: 
The number of soldiers for each row is: 
row 0 -> 2 
row 1 -> 4 
row 2 -> 1 
row 3 -> 2 
row 4 -> 5 
Rows ordered from the weakest to the strongest are [2,0,3,1,4]
```

**Example 2:**

```
Input: mat = 
[[1,0,0,0],
 [1,1,1,1],
 [1,0,0,0],
 [1,0,0,0]], 
k = 2
Output: [0,2]
Explanation: 
The number of soldiers for each row is: 
row 0 -> 1 
row 1 -> 4 
row 2 -> 1 
row 3 -> 1 
Rows ordered from the weakest to the strongest are [0,2,3,1]
```

**Constraints:**

* `m == mat.length`
* `n == mat[i].length`
* `2 <= n, m <= 100`
* `1 <= k <= m`
* `matrix[i][j]` is either 0 **or** 1.

其实这题就一个top k问题。题目给了条件说，1会聚集在左边，所以其实中间搜的时候，可以用二分，但太懒了，所以就只用了一个个搜。T:O(n\*mlogk)，S:O(n)。

另外再补一个比较有趣的看问题方法，因为1都在左边，所以我们可以vertical地扫，扫到是0而且左边是1，那么证明这个是这一行第一个0。那么我们就可以把这个加到结果里。T:O(mn), S:O(1)

```java
public int[] kWeakestRows(int[][] mat, int k) {
    if (mat == null || mat.length == 0 || mat[0].length == 0 || k < 1 || k > mat.length) {
        return null;
    }

    // Pair<1's count, row num>
    PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<>(k, (p1, p2) -> {
        if (p1.getKey() == p2.getKey()) {
            return p1.getValue().compareTo(p2.getValue());
        } else {
            return p1.getKey().compareTo(p2.getKey());
        }
    });

    // count ones then put in heap
    for (int row = 0; row < mat.length; row++) {
        int oneCnt = 0;
        for (int col = 0; col < mat[row].length; col++) {
            if (mat[row][col] == 1) {
                oneCnt++;
            }                
        }

        Pair p = new Pair(oneCnt, row);
        pq.offer(p);
    }

    int[] res = new int[k];
    // pull top k
    for (int i = 0; i < res.length; i++) {
        res[i] = pq.poll().getValue();
    }

    return res;
}

// leetcode solution vertical 解法
public int[] kWeakestRows(int[][] mat, int k) {

    int m = mat.length;
    int n = mat[0].length;

    int [] indexes = new int[k];
    int nextInsertIndex = 0;

    // This code does the same as the animation above.
    for (int c = 0; c < n && nextInsertIndex < k; c++) {
        for (int r = 0; r < m && nextInsertIndex < k; r++) {
            // If this is the first 0 in the current row.
            if (mat[r][c] == 0 && (c == 0 || mat[r][c - 1] == 1)) {
                indexes[nextInsertIndex] = r;
                nextInsertIndex++;
            }
        }
    }

    /* If there aren't enough, it's because some of the first k weakest rows
     * are entirely 1's. We need to include the ones with the lowest indexes
     * until we have at least k. */
    for (int r = 0; nextInsertIndex < k ; r++) {
        /* If index i in the last column is 1, this was a full row and therefore
         * couldn't have been included in the output yet. */
        if (mat[r][n - 1] == 1) {
            indexes[nextInsertIndex] = r;
            nextInsertIndex++;
        }
    }

    return indexes;

}
```
