# L593 Stone Game II

There is a stone game.At the beginning of the game the player picks n piles of stones`in a circle`.

The goal is to merge the stones in one pile observing the following rules:

At each step of the game,the player can merge two adjacent piles to a new pile.\
The score is the number of stones in the new pile.\
You are to determine the minimum of the total score.

**Example**

For`[1, 4, 4, 1]`, in the best solution, the total score is`18`:

```
1. Merge second and third piles => [2, 4, 4], score +2
2. Merge the first two piles => [6, 4]，score +6
3. Merge the last two piles => [10], score +10
```

Other two examples:\
`[1, 1, 1, 1]`return`8`\
`[4, 4, 5, 9]`return`43`

这题有环，用repeat来破环。

```java
public int stoneGame2(int[] A) {
    if (A == null || A.length == 0) {
        return 0;
    }

    // break loop by 2n
    int n = A.length;
    int[][] dp = new int[n * 2][n * 2];
    int[] sum = new int[n * 2 + 1];

    // don't need to actually duplicate A, use mod to get the right value
    for (int i = 1; i <= 2 * n; i++) {
        sum[i] = sum[i - 1] + A[(i - 1) % n];
    }

    // init dp[i][i] to 0
    for (int i = 0; i < 2 * n; i++) {
        dp[i][i] = 0;
    }

    for (int len = 2; len <= 2 * n; len++) {
        for (int s = 0; s + len - 1 < 2 * n; s++) {
            int e = s + len - 1;
            dp[s][e] = Integer.MAX_VALUE;
            for (int k = s; k < e; k++) {// find in the range, not including end point
                dp[s][e] = Math.min(dp[s][e], dp[s][k] + dp[k + 1][e] + sum[e + 1] - sum[s]);
            }
        }
    }

    // need to loop through i to i + n - 1 to find min, because those are all possible way to break the circle
    int ans = Integer.MAX_VALUE;
    for (int i = 0; i < n; i++) {
        ans = Math.min(ans, dp[i][i + n - 1]);
    }

    return ans;
}
```
