# L396 Coins in line III

There are\_n\_coins in a line. Two players take turns to take a coin from one of the ends of the line until there are no more coins left. The player with the larger amount of money wins.

Could you please decide the first player will win or lose?

**Example**

Given array A =`[3,2,2]`, return`true`.

Given array A =`[1,2,4]`, return`true`.

Given array A =`[1,20,4]`, return`false`.

[**Challenge**](http://www.lintcode.com/en/problem/coins-in-a-line-iii/#challenge)

Follow Up Question:

If n is even. Is there any\_hacky\_algorithm that can decide whether first player will win or lose in O(1) memory and O(n) time?

2个字，没懂...具体看笔记...

迭代版：还是考虑**当前**取硬币的人最优

```java
public boolean firstWillWin(int[] values) {
    if (values == null || values.length == 0) {
        return false;
    }

    int n = values.length;
    int[] sum = new int[n + 1];
    // use suffix sum to quicky calculate coins value sum range from i to j
    for (int i = 1; i <= n; i++) {
        sum[i] = sum[i - 1] + values[i - 1];
    }

    int[][] dp = new int[n][n];
    // init dp diagonal with coins values
    for (int i = 0; i < n; i++) {
        dp[i][i] = values[i];
    }

    // fill the rest of dp using len, 
    // becasue we need to fill the right upper side of the matrix, 
    // so can't loop as usual
    for (int len = 2; len <= n; len++) {
        // iterate through all starting point
        for (int s = 0; s + len <= n; s++) {
            int e = s + len - 1;
            int curSum = sum[e + 1] - sum[s];
            dp[s][e] = curSum - Math.min(dp[s + 1][e], dp[s][e - 1]);
        }
    }

    return dp[0][n - 1] > sum[n] / 2;
}
```

memorize版：

```java
public boolean firstWillWin(int[] values) {
    // write your code here
    int n = values.length;
    int [][]dp = new int[n + 1][n + 1];
    boolean [][]flag =new boolean[n + 1][n + 1];
    int[][] sum = new int[n + 1][n + 1];
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            sum[i][j] = i == j ? values[j] : sum[i][j-1] + values[j];
        }
    }
    int allsum = 0;
    for(int now : values) 
        allsum += now;

    return allsum < 2*MemorySearch(0,values.length - 1, dp, flag, values, sum);
}
int MemorySearch(int left, int right, int [][]dp, boolean [][]flag, int []values, int [][]sum) {
    if(flag[left][right])   
        return dp[left][right];

    flag[left][right] = true;
    if(left > right) {
        dp[left][right] = 0;
    } else if (left == right) {
        dp[left][right] = values[left];
    } else if(left + 1 == right) {
        dp[left][right] = Math.max(values[left], values[right]);
    } else {
        int cur = Math.min(MemorySearch(left+1, right, dp, flag, values, sum), MemorySearch(left,right-1, dp, flag, values, sum));
        dp[left][right] = sum[left][right] - cur;
    }
    return dp[left][right];   
}
```
