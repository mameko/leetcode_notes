# 265 Paint House II

There are a row ofnhouses, each house can be painted with one of thekcolors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a`nxk`cost matrix. For example,`costs[0][0]`is the cost of painting house 0 with color 0;`costs[1][2]`is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

**Note:**\
All costs are positive integers.

**Follow up:**\
Could you solve it inO(nk) runtime?

这题得看答案才想出nk的算法。如果按照[Paint House 1](256-paint-house.md)的做法的话，每次我们都得找除了当前房子这个颜色之外颜色的min cost。这得话O（k）的时间，这会导致复杂度变为O(nk^2)。在网上看到大神用了[Product Except itself](../19-left-to-right-to-left-get-best/l50-product-of-array-exclude-itself.md)的思想，花费点空间每次把最小值存到一个数组里，下一轮算的时候不用每个扫一遍。后来，在discuss上也看到了constant space的解法。直接改costs数组上的数，然后做的时候把最小min，min的下标和次小secendMin存起来，下一轮的时候，如果颜色不同，我们用min，如果发现颜色相同的话，我们用次小。也放上来作为参考。

```java
public int minCostII(int[][] costs) {
    if (costs == null || costs.length == 0 || costs[0].length == 0) {
        return 0;
    }

    int n = costs.length;
    int k = costs[0].length;
    int[][] dp = new int[n][k];

    int[] minExceptItself = new int[k];

    for (int i = 0; i < n; i++) {

        for (int j = 0; j < k; j++) {
            dp[i][j] = costs[i][j] + minExceptItself[j];
        }

        int[] left = new int[k + 1];
        int[] right = new int[k + 1];

        left[0] = Integer.MAX_VALUE;
        right[k] = Integer.MAX_VALUE;

        for (int l = 1; l <= k; l++) {
            left[l] = Math.min(left[l - 1], dp[i][l - 1]);
        }

        for (int r = k - 1; r >= 0; r--) {
            right[r] = Math.min(right[r + 1], dp[i][r]);
        }

        for (int e = 0; e < k; e++) {
            minExceptItself[e] = Math.min(left[e], right[e + 1]);
        }
    }

    int result = Integer.MAX_VALUE;
    for (int i = 0; i < k; i++) {
        result = Math.min(result, dp[n - 1][i]);
    }

    return result;
}
```

constant space解法：

```java
public int minCostII(int[][] costs) {
    if (costs == null || costs.length == 0 || costs[0].length == 0) {
        return 0;
    }

    int n = costs.length;
    int m = costs[0].length;
    int min1Loc = -1;
    int min2Loc = -1;

    for (int i = 0; i < n; i++) {
        int last1Loc = min1Loc;
        int last2Loc = min2Loc;
        min1Loc = -1;
        min2Loc = -1;
        for (int j = 0; j < m; j++) {                
            if (last1Loc != j) {
                costs[i][j] += i == 0 ? 0 : costs[i - 1][last1Loc];
            } else {
                costs[i][j] += i == 0 ? 0 : costs[i - 1][last2Loc];
            }

            if (min1Loc < 0 || costs[i][j] < costs[i][min1Loc]) {
                min2Loc = min1Loc;                
                min1Loc = j;                
            } else if (min2Loc < 0 || costs[i][j] < costs[i][min2Loc]) {
                min2Loc = j;
            }
        }
    }       

    return costs[n - 1][min1Loc];
}
```
