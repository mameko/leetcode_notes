# 750 Number Of Corner Rectangles

Given a grid where each entry is only 0 or 1, find the number of corner rectangles.

A _corner rectangle_ is 4 distinct 1s on the grid that form an axis-aligned rectangle. Note that only the corners need to have the value 1. Also, all four 1s used must be distinct.

**Example 1:**

```
Input: grid = 
[[1, 0, 0, 1, 0],
 [0, 0, 1, 0, 1],
 [0, 0, 0, 1, 0],
 [1, 0, 1, 0, 1]]
Output: 1
Explanation: There is only one corner rectangle, with corners grid[1][2], grid[1][4], grid[3][2], grid[3][4].
```

**Example 2:**

```
Input: grid = 
[[1, 1, 1],
 [1, 1, 1],
 [1, 1, 1]]
Output: 9
Explanation: There are four 2x2 rectangles, four 2x3 and 3x2 rectangles, and one 3x3 rectangle.
```

**Example 3:**

```
Input: grid = 
[[1, 1, 1, 1]]
Output: 0
Explanation: Rectangles must have four distinct corners.
```

**Note:**

1. The number of rows and columns of `grid` will each be in the range `[1, 200]`.
2. Each `grid[i][j]` will be either `0` or `1`.
3. The number of `1`s in the grid will be at most `6000`.

Hint ： For each pair of 1s in the new row (say at `new_row[i]` and `new_row[j]`), we could create more rectangles where that pair forms the base. The number of new rectangles is the number of times some previous row had `row[i] = row[j] = 1`.

这题，不看hint完全没想法。看了答案也花了很长时间理解。其实，brute force的实现就是这样，每次找下一行。找的时候，发现了1，就去找同行里其他的1，找到两个1之后，往上找，看上面有没有同样位置也是1的。这里用了hashmap来存，每次加完记得更新hashmap。编码方式跟graph Queue的一样，i \* m + j。T:O(R\*C^2)，S:O(C^2)

```java
public int countCornerRectangles(int[][] grid) {
    if (grid == null || grid.length < 2 || grid[0].length == 0) {
        return 0;
    }

    int rows = grid.length;
    int cols = grid[0].length;

    Map<Integer, Integer> count = new HashMap<>();   
    int ans = 0;
    for (int i = 0; i < rows; i++) {
        for (int j1 = 0; j1 < cols; j1++) {
            if (grid[i][j1] == 1) {
                for (int j2 = j1 + 1; j2 < cols; j2++) {
                    if (grid[i][j2] == 1) {
                        int pos = j1 * cols + j2;
                        int cnt = count.getOrDefault(pos, 0);
                        ans += cnt;
                        count.put(pos, cnt + 1);
                    }
                }
            }

        }
    }

    return ans;
}
```
