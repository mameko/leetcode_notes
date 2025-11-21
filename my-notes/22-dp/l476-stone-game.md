# L476 Stone Game

There is a stone game.At the beginning of the game the player picks`n`piles of stones in a line.

The goal is to merge the stones in one pile observing the following rules:

1. At each step of the game,the player can merge two adjacent piles to a new pile.
2. The score is the number of stones in the new pile.

You are to determine the minimum of the total score.

**Example**

For`[4, 1, 1, 4]`, in the best solution, the total score is`18`:

```
1. Merge second and third piles => [4, 2, 4], score +2
2. Merge the first two piles => [6, 4]，score +6
3. Merge the last two piles => [10], score +10
```

Other two examples:\
`[1, 1, 1, 1]`return`8`\
`[4, 4, 5, 9]`return`43`

要分开算sum。

```java
public int stoneGame(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }

    int n = A.length;
    int[][] dp = new int[n][n];
    int[] sum = new int[n + 1];
    // because we want to get min, so init to MAX
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            dp[i][j] = Integer.MAX_VALUE;
        }
    }
    // init diagonal
    for (int i = 0; i < n; i++) {
        dp[i][i] = 0;
        sum[i + 1] = sum[i] + A[i];
    }

    // mem search
    calRange(A, 0, n - 1, dp, sum);
    return dp[0][n - 1];
}

private int calRange(int[] A, int s, int e, int[][] dp, int[] sum) {

    // we calculate the range before, so just return it
    // note : don't try to use dp[][] > 0, because 0 is also a valid value
    if (dp[s][e] != Integer.MAX_VALUE) { 
        return dp[s][e];
    }

    // if we never calculate, we calculate here
    int min = Integer.MAX_VALUE;
    int rangeSum = sum[e + 1] - sum[s];
    for (int i = s; i < e; i++) {
        int cur = 0;
        int r1 = calRange(A, s, i, dp, sum);
        int r2 = calRange(A, i + 1, e, dp, sum);

        cur = r1 + r2 + rangeSum;
        min = Math.min(min, cur);
    }
    dp[s][e] = min;
    return dp[s][e];
}
```
