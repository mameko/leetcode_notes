# L395 Coins in Line II

There are n coins with different value in a line. Two players take turns to take one or two coins from left side until there are no more coins left. The player who take the coins with the most value wins.

Could you please decide the **first** player will win or lose?

**Example**

Given values array A =`[1,2,2]`, return`true`.

Given A =`[1,2,4]`, return`false`.

这次算的是是否得到一半以上的价值。具体解释看笔记，其实不太懂。

迭代dp：考虑的是**当前**取硬币的人最后最优能取多少

```java
public boolean firstWillWin(int[] values) {
    if (values == null || values.length == 0) {
        return false;
    } else if (values.length < 3) {
        return true;
    }

    int n = values.length;
    int[] sum = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        sum[i] = sum[i - 1] + values[n - i];
    }
    // because 1st player can choose take 1 or 2, so can take max (sum[i] - dp[i - 1], sum[i] - dp[i - 2])
    int[] dp = new int[n + 1];// dp[i] means currently i coins left, max current player can get
    dp[1] = values[n - 1];// 1 coin left, current player take it to get max
    dp[2] = values[n - 1] + values[n - 1];// 2 coins left, current player take both to get max
    for (int i = 3; i < dp.length; i++) {
        dp[i] = Math.max(sum[i] - dp[i - 1], sum[i] - dp[i - 2]);
    }

    return dp[n] > sum[n] / 2;
}
```

memorize dp：

```java
public boolean firstWillWin(int[] values) {
    // write your code here
    int n = values.length;
    int []dp = new int[n + 1];
    boolean []flag =new boolean[n + 1];
    int []sum = new int[n+1];
    int allsum = values[n-1];
    sum[n-1] = values[n-1];
    for(int i = n-2; i >= 0; i--) { 
        sum[i] += sum[i+1] + values[i];
        allsum += values[i];
    }
    return allsum/2 < MemorySearch(0, n, dp, flag, values, sum);
}

int MemorySearch(int i, int n, int []dp, boolean []flag, int []values, int []sum) {
    if(flag[i] == true)
        return dp[i];
    flag[i] = true;
    if(i == n)  {
        dp[n] = 0;  
    } else if(i == n-1) {
        dp[i] = values[i];
    } else if(i == n-2) {
        dp[i] = values[i] + values[i + 1]; 
    } else {
        dp[i] = sum[i] -
            Math.min(MemorySearch(i+1, n, dp, flag, values, sum) , MemorySearch(i+2, n, dp, flag, values, sum));
    }
    return dp[i];
}
```
