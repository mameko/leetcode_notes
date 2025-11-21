# L574 Build Post Office

Given a 2D grid, each cell is either an house`1`or empty`0`(the number zero, one), find the place to build a post office, the distance that post office to all the house sum is smallest. Return the smallest distance. Return`-1`if it is not possible.

## Notice

* You can pass through house and empty.
* You only build post office on an empty.

**Example**

Given a grid:

```
0 1 0 0
1 0 1 1
0 1 0 0
```

return`6`. (Placing a post office at (1,1), the distance that post office to all the house sum is smallest.)

这题完爆朕的脑容量，只能把答案写下来以后慢慢研究。T\_T...好像是把O(mnk)优化成O(nmlog(k))

```java
public int shortestDistance(int[][] grid) {
    if (grid == null || grid.length == 0 || grid[0].length == 0) {
        return -1;
    }

    int n = grid.length;
    int m = grid[0].length;

    ArrayList<Integer> rows = new ArrayList<>();
    ArrayList<Integer> cols = new ArrayList<>();
    // putting all the manhatan distance point for later calculation
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                rows.add(i);
                cols.add(j);
            }
        }
    }

    Collections.sort(rows);// becasue we add them according to rows, so rows will be already in order 
    Collections.sort(cols);

    // prefix sum
    ArrayList<Integer> preSumX = new ArrayList<>();
    ArrayList<Integer> preSumY = new ArrayList<>();
    preSumX.add(0);
    preSumY.add(0);
    int houseNum = rows.size();
    for (int i = 0; i < houseNum; i++) {
        preSumX.add(preSumX.get(i) + rows.get(i));
        preSumY.add(preSumY.get(i) + cols.get(i));
    }

    // loop through all the 0 location and calculate the min
    int min = Integer.MAX_VALUE;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 0) {
                // int cur = calculate(rows, i) + calculate(cols, j);
                int cur = getCost(rows, preSumX, i, houseNum) + getCost(cols, preSumY, j, houseNum);
                min = Math.min(min, cur);
            }
        }
    }
    return min;

}

/*
 * x1, x2, x3, x, x4 Remove abs : #ofhouse<x * x - presum[#ofhouse < x]
 * presum[#ofhouse > x] - #ofhouse>x * x => 3 * x - (x1 + x2 + x3) + x4 - x
 * => (3 * pos - sum[3]) + (sum[4] - sum[3] - pos)
 * 
 * do binary search on x (pos), find 1st location less than or equal to x
 */
private int getCost(ArrayList<Integer> rOrc, ArrayList<Integer> preSum, int pos, int n) {
    if (n == 0) {
        return 0;
    }

    if (rOrc.get(0) > pos) { // all house are on the right of pos
        return preSum.get(n) - n * pos;
    }

    int l = 0;
    int r = n - 1;
    int index = 0;
    while (l + 1 < r) {
        int mid = l + (r - l) / 2;
        if (rOrc.get(mid) <= pos) {
            l = mid;
        } else if (rOrc.get(mid) > pos) {
            r = mid;
        } 
    }

    if (rOrc.get(r) <= pos) {// also need to return in case of equal
        index = r;
    } else {
        index = l;
    }

    return preSum.get(n) - preSum.get(index + 1) - pos * (n - (index + 1)) + (index + 1) * pos - preSum.get(index + 1);

}
/* TEL
private int calculate(ArrayList<Integer> locs, int i) {
    int res = 0;
    for (Integer loc : locs) {
        res += Math.abs(loc - i);
    }
    return res;
}
*/
```
