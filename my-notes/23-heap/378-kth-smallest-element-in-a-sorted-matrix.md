# 378 Kth Smallest Element in a Sorted Matrix

Find the\_k\_th smallest number in at row and column sorted matrix.

**Example**

Given k =`4`and a matrix:

```
[
  [1 ,5 ,7],
  [3 ,7 ,8],
  [4 ,8 ,9],
]
```

return`5`

[**Challenge**](http://www.lintcode.com/en/problem/kth-smallest-number-in-sorted-matrix/#challenge)

Solve it in O(k log n) time where n is the bigger one between row size and column size.

呢题仲有另一种解法，binary search value range。大概做法系：start = \[0,0], end = \[n, m] + 1。因为行/列有序，所以最大最小的范围确定。算出mid，然后数数小于等于mid的有多少，如果小于k的话，start = mid，找后半。反之找前半。因为行/列有序，感觉可以用[240 Search in a 2D matrix II](../17-binary-search/240-search-in-a-2d-matrix-ii.md)的方法？这种二分的找法跟quick select有点像，另外树里的 [230 Kth Smallest Element in a BST](../26-tree/230-kth-smallest-element-in-a-bst.md) 也很像。具体代码有空再补。

这题因为矩阵是有序的，所以我们可以从最小的开始考虑，每次把次小的找出来，从而找到第k小。这里有点层遍历的感觉，每次把下一层的入队，这里用的是priorityqueue，因为每次要找队里最小的（邻居中最小的）。一直找找找，直到找够k个为止。

```java
public int kthSmallest(int[][] matrix, int k) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0 || k < 0) {
        return Integer.MAX_VALUE;
    }

    Queue<Loc> pq = new PriorityQueue<>();
    HashSet<Loc> visited = new HashSet<>();
    LinkedList<Integer> res = new LinkedList<>();
    Loc first = new Loc(0, 0, matrix[0][0]);
    pq.offer(first);
    visited.add(first);
    while (res.size() < k) {
        Loc tmp = pq.poll();
        res.add(tmp.val);
        int nextI = tmp.i + 1;
        int nextJ = tmp.j + 1;

        if (nextI < matrix.length) {
            Loc next1 = new Loc(nextI, tmp.j, matrix[nextI][tmp.j]);
            if (!visited.contains(next1)) {
                pq.offer(next1);
                visited.add(next1);
            }
        }

        if (nextJ < matrix[tmp.i].length) {
            Loc next2 = new Loc(tmp.i, nextJ, matrix[tmp.i][nextJ]);
            if (!visited.contains(next2)) {
                pq.offer(next2);
                visited.add(next2);
            }
        }
    }
    return res.getLast();
}

class Loc implements Comparable<Loc> {
    int i;
    int j;
    int val;

    public Loc(int iLoc, int jLoc, int v) {
        i = iLoc;
        j = jLoc;
        val = v;
    }

    public int compareTo(Loc o) {
        return val - o.val;
    }

    @Override
    public String toString() {
        return String.valueOf(val);
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Loc) {
            Loc tmp = (Loc) obj;
            return i == tmp.i && j == tmp.j && val == tmp.val;
        }

        return false;
    }

    @Override
    public int hashCode() {
        return Objects.hash(i, j, val);
    }
}
```

简洁d版：

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0 || k < 0) {
            return -1;
        }

        int n = matrix.length;
        int m = matrix[0].length;

        PriorityQueue<Loc> minHeap = new PriorityQueue<>(k, new Comparator<Loc>(){
            public int compare(Loc l1, Loc l2){
                return l1.val - l2.val;
            }
        });

        boolean[][] visited = new boolean[n][m];
        LinkedList<Integer> res = new LinkedList<>();
        minHeap.offer(new Loc(0, 0, matrix[0][0]));
        visited[0][0] = true;

        while (res.size() < k) {
            Loc cur = minHeap.poll();
            res.add(cur.val);

            int nexti = cur.i + 1;
            int nextj = cur.j + 1;

            if (nextj < m && !visited[cur.i][nextj]) {
                minHeap.offer(new Loc(cur.i, nextj, matrix[cur.i][nextj]));
                visited[cur.i][nextj] = true;                    
            }

            if (nexti < n && !visited[nexti][cur.j]) {
                minHeap.offer(new Loc(nexti, cur.j, matrix[nexti][cur.j]));
                visited[nexti][cur.j] = true;
            }            
        }

        return res.getLast();
    }
}

class Loc {
    int i;
    int j;
    int val;

    public Loc(int i, int j, int v) {
        this.i = i;
        this.j = j;
        this.val = v;
    }
}
```
