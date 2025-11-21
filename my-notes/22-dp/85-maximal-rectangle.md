# 85 Maximal Rectangle

Given a 2D`boolean`matrix filled with`False`and`True`, find the largest rectangle containing all`True`and return its area.

**Example**

Given a matrix:

```
[
  [1, 1, 0, 0, 1],
  [0, 1, 0, 0, 1],
  [0, 0, 1, 1, 1],
  [0, 0, 1, 1, 1],
  [0, 0, 0, 0, 1]
]
```

return`6`.

这题做法是，先把每一行看成一个直方图。然后用直方图的方法来解。

```java
public int maximalRectangle(boolean[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return 0;
    }

    int m = matrix.length;
    int n = matrix[0].length;

    int[] curRowHis = new int[n];
    int max = Integer.MIN_VALUE;

    // T: O(mn)
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (!matrix[i][j]) {
                curRowHis[j] = 0; 
            } else {
                curRowHis[j] += 1;
            }
        }
        int curMax = histogram(curRowHis);
        max = Math.max(curMax, max);
    }

    return max;
}

private int histogram(int[] A) {
        int max = Integer.MIN_VALUE;
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i <= A.length; i++) {
            int cur = (i == A.length) ? -1 : A[i];
            while (!stack.isEmpty() && cur <= A[stack.peek()]) {
                int h = A[stack.pop()];
                int w = stack.isEmpty() ? i : i - stack.peek() - 1;
                int area = h * w;
                max = Math.max(max, area);
            }

            stack.push(i);
        }

        return max;
    }
```
