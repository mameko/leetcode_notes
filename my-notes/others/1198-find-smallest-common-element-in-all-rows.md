# 1198 Find Smallest Common Element in All Rows

Given an `m x n` matrix `mat` where every row is sorted in **strictly** **increasing** order, return _the **smallest common element** in all rows_.

If there is no common element, return `-1`.

**Example 1:**

```
Input: mat = [[1,2,3,4,5],[2,4,5,8,10],[3,5,7,9,11],[1,3,5,7,9]]
Output: 5
```

**Example 2:**

```
Input: mat = [[1,2,3],[2,3,4],[2,3,5]]
Output: 2
```

**Constraints:**

* `m == mat.length`
* `n == mat[i].length`
* `1 <= m, n <= 500`
* `1 <= mat[i][j] <= 104`
* `mat[i]` is sorted in strictly increasing order.

这题瞧了半天，因为排序，还以为跟[74 Search in a 2D Matrix](../17-binary-search/74-search-in-a-2d-matrix.md)有关系。又或者跟[L486 Merge K sorted Arrays](../merge-sort/l486-merge-k-sorted-arrays.md)有关系，拿着heap想了半天，实现起来还是很复杂。然后忍不住看提示。才发现，原来，每一行的数字是unique的。题目写着strickly increasing。所以其实只要数数字频率就可以了。然后再判断个min就ok了。反而跟之前[1394 Find Lucky Integer in an Array](1394-find-lucky-integer-in-an-array.md)比较像。T:O(mn), S:O(num range)

```java
public int smallestCommonElement(int[][] mat) {
    if (mat == null || mat.length == 0 || mat[0].length == 0) {
        return -1;
    }

    Map<Integer, Integer> numToCnt = new HashMap<>();
    for (int i = 0; i < mat.length; i++) {
        for (int j = 0; j < mat[i].length; j++) {
            int cur = mat[i][j];
            numToCnt.put(cur, numToCnt.getOrDefault(cur, 0) + 1);
        }
    }

    int rows = mat.length;
    int min = Integer.MAX_VALUE;
    for (Map.Entry<Integer, Integer> entry : numToCnt.entrySet()) {
        if (entry.getValue() == rows) {
            min = Math.min(min, entry.getKey());
        }
    }

    return min == Integer.MAX_VALUE ? -1 : min;
}
```
