# 276 Paint Fence

There is a fence with`n`posts, each post can be painted with one of the`k`colors.\
You have to paint all the posts such that no more than two adjacent fence posts have the same color.\
Return the total number of ways you can paint the fence.

## Notice

`n`and`k`are non-negative integers.

**Example**

Given`n`=3,`k`=2 return 6

```
      post 1,   post 2, post 3
way1    0         0       1 
way2    0         1       0
way3    0         1       1
way4    1         0       0
way5    1         0       1
way6    1         1       0
```

看注释...

```java
public int numWays(int n, int k) {
    /* state : dp[i][0] - pick same color with previous post
               dp[i][1] - pick different color with previous post

       fun   : dp[i][0] = dp[i - 1][1] - becasue we can't have 3 post with same color, so we need to pick previous post(i - 1) not having same color with post (i - 2), that is dp[i - 1][1]
             : dp[i][1] = (k - 1) * (dp[i - 1][0] + dp[i - 1][1]) - becasue we are picking a color different from previous one, so we can choose from k - 1 color, in all the ways i - 1 can choose(dp[i - 1][0] + dp[i - 1][1]).
       init : dp[1][0] = 0 - becasue no previous post exist
              dp[1][1] = k - first post can pick k different color with the none existing post 0

       ans  : dp[n][0] + dp[n][1] - total way of painting the n post same with n - 1 & different with n - 1
    */
    // int[][] dp = new int[n + 1][2];
    int[][] dp = new int[2][2];
    dp[1][0] = 0;
    dp[1][1] = k;
    for (int i = 2; i <= n; i++) {
        dp[i % 2][0] = dp[(i - 1) % 2][1];
        dp[i % 2][1] = (k - 1) * (dp[(i - 1) % 2][0] + dp[(i - 1) % 2][1]); 
    }

    return dp[n % 2][0] + dp[n % 2][1];
}
```
