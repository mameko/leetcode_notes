# 351 Android Unlock Patterns

Given an Android**3x3**key lock screen and two integers**m**and**n**, where 1 ≤ m ≤ n ≤ 9, count the total number of unlock patterns of the Android lock screen, which consist of minimum of**m**keys and maximum**n**keys.

**Rules for a valid pattern:**

1. Each pattern must connect at least **m** keys and at most **n**keys.
2. All the keys must be distinct.
3. If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
4. The order of keys used matters.

![](https://leetcode.com/static/images/problemset/android-unlock.png)

**Explanation:**

```
| 1 | 2 | 3 |
| 4 | 5 | 6 |
| 7 | 8 | 9 |
```

**Invalid move:**`4 - 1 - 3 - 6`\
Line 1 - 3 passes through key 2 which had not been selected in the pattern.

**Invalid move:**`4 - 1 - 9 - 2`\
Line 1 - 9 passes through key 5 which had not been selected in the pattern.

**Valid move:**`2 - 4 - 1 - 3 - 6`\
Line 1 - 3 is valid because it passes through key 2, which had been selected in the pattern

**Valid move:**`6 - 5 - 4 - 1 - 9 - 2`\
Line 1 - 9 is valid because it passes through key 5, which had been selected in the pattern.

**Example:**\
Given**m**= 1,**n**= 1, return 9.

这题不看答案完全想不到，感觉比较backtrack而不是DP。首先把要所有要跳过的数字放进pass里(在进入下一步时要用)。然后带上一个visited数组一边递归一边backtrack。这里因为对称的关系，所以从1开始的，会跟3，7，9的重复，所以乘以4，同理从2开始的会跟4，6，8重复再×4.最后从5开始的只有一种所以加上就ok了。递归函数里我们数着还需要多少个数字。如果需要的数字remain==0，我们不需要继续递归下去，返回1（表示我们找到了一种合法的输入了）。然后如果现在剩下要找的数字大于0的话，我们从1到9里找下一个数字。递归前先把visited设好，递归后记得backtrack。进入递归的条件是下一个数字还没用过，而且这个数字到下一个数字之间的间距为0或者跳过一个数字而且那个数字已经出现过。如果符合条件的话，我们把剩下要找的数字数目减一，从下一个数字开始继续递归。感觉就像permutation的模板，每次要把所有合法的数字loop一次，把没用过的尽量用上。

```java
public int numberOfPatterns(int m, int n) {

    int[][] pass = new int[10][10];
    pass[1][3] = pass[3][1] = 2;
    pass[3][9] = pass[9][3] = 6;
    pass[7][9] = pass[9][7] = 8;
    pass[1][7] = pass[7][1] = 4;
    pass[4][6] = pass[6][4] = 5;
    pass[2][8] = pass[8][2] = 5;
    pass[1][9] = pass[9][1] = pass[3][7] = pass[7][3] = 5;

    boolean[] visited = new boolean[10];
    int res = 0;
    for (int i = m; i <= n; i++) {
        res += helper(1, visited, pass, i - 1) * 4;
        res += helper(2, visited, pass, i - 1) * 4;
        res += helper(5, visited, pass, i - 1);
    }
    return res;
}

private int helper(int num, boolean[] visited, int[][] pass, int remain) {
    if (remain == 0) {
        return 1;
    }

    visited[num] = true;

    int res = 0;
    for (int next = 1; next <= 9; next++) {
        int p = pass[num][next];
        if (!visited[next] && (p == 0 || visited[p])) {
            res += helper(next, visited, pass, remain - 1);
        }
    }

    visited[num] = false;
    return res;
}
```
