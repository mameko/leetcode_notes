# L390 Find Peak Element II

There is an integer matrix which has the following features:

* The numbers in adjacent positions are different.
* The matrix has n rows and m columns.
* For all i < m,_A\[0]\[i] < A\[1]\[i] && A\[n - 2]\[i] > A\[n - 1]\[i]_.
* For all j < n,_A\[j]\[0] < A\[j]\[1] && A\[j]\[m - 2] > A\[j]\[m - 1]_.

We define a position P is a peek if:

```
A[j][i] > A[j+1][i] && A[j][i] > A[j-1][i] && 
A[j][i] > A[j][i+1] && A[j][i] > A[j][i-1]
```

Find a peak element in this matrix. Return the index of the peak.

## Notice

The matrix may contains multiple peeks, find any of them.

**Example**

Given a matrix:

```
[
  [1 ,2 ,3 ,6 ,5],
  [16,41,23,22,6],
  [15,17,24,21,7],
  [14,18,19,20,10],
  [13,14,11,10,9]
]
```

return index of 41 (which is`[1,1]`) or index of 24 (which is`[2,2]`)

[**Challenge**](http://www.lintcode.com/en/problem/find-peak-element-ii/#challenge)

Solve it in O(_n+m_) time.

If you come up with an algorithm that you _thought it is O(n log m) or O(m log n), can you prove it is actually O(n+m_) or propose a similar but O(_n+m_) algorithm?

这题是上一题的follow up，在2D矩阵找peak。是peak的条件是，比4周的高，所以从4周找起。而且每次vertical和horizontal地把找的范围缩小，才能做到O(_n+m_)。

```java
public List<Integer> findPeakII(int[][] A) {
    if (A == null || A.length == 0) {
        return null;
    }
    int n = A.length; // x going down
    int m = A[0].length; // y going right
    // cut squre in half horizontally, so pass in true;
    // then cut square in half, vertically to shrink the peak position.
    return findInRange(1, 1, n - 2, m - 2, A, true);
}

private List<Integer> findInRange(int x1, int y1, int x2, int y2, 
                                  int[][] A, boolean horizontal) {
    if (horizontal) { // cut square in half horizontally
        int mid = x1 + (x2 - x1) / 2;
        // use max Index to keep track of max elem's index in the row
        int maxInd = y1; 
        for (int i = y1; i <= y2; i++) {// loop to find max
            if (A[mid][i] > A[mid][maxInd]) {
                maxInd = i;
            }
        }

        if (A[mid][maxInd] < A[mid - 1][maxInd]) {
            // the max elem in this row smaller than it's top, 
            // means peak elem can be found in upper half
            // next time cut it vertically
            return findInRange(x1, y1, mid - 1, y2, A, false);
        } else if (A[mid][maxInd] < A[mid + 1][maxInd]) {
            // the max elem in this row smaller than it's bottom, 
            // means peak elem can be found in lower half
            // next time cut it vertically
            return findInRange(mid + 1, y1, x2, y2, A, false);
        } else { // the max elem is the peak;
            return new ArrayList<Integer>(Arrays.asList(mid, maxInd));
        }

    } else { // cut square in half vertically
        int mid = y1 + (y2 - y1) / 2;
        int maxInd = x1;
        for (int i = x1; i <= x2; i++) {
            if (A[maxInd][mid] < A[i][mid]) {
                maxInd = i;
            }
        }

        if (A[maxInd][mid] < A[maxInd][mid - 1]) {
            // the max elem in this col smaller than it's left, 
            // means peak can be found in the left half
            return findInRange(x1, y1, x2, mid - 1, A, true);
        } else if (A[maxInd][mid] < A[maxInd][mid + 1]) {
            // the max elem in this col is smaller than it's right, 
            // means peak can be found in the right half
            return findInRange(x1, mid + 1, x2, y2, A, true);
        } else { // the max elem is the peak;
            return new ArrayList<Integer>(Arrays.asList(maxInd, mid));
        }
    }
}
```
