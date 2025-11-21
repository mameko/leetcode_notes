# 474 Ones and Zeroes

You are given an array of binary strings `strs` and two integers `m` and `n`.

Return _the size of the largest subset of `strs` such that there are **at most**_ `m` `0`_'s and_ `n` `1`_'s in the subset_.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

**Example 1:**

```
Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.
```

**Example 2:**

```
Input: strs = ["10","0","1"], m = 1, n = 1
Output: 2
Explanation: The largest subset is {"0", "1"}, so the answer is 2.
```

**Constraints:**

* `1 <= strs.length <= 600`
* `1 <= strs[i].length <= 100`
* `strs[i]` consists only of digits `'0'` and `'1'`.
* `1 <= m, n <= 100`

这题一看，就觉得像[322 coin change](322-coin-change.md)。觉得是dp，可是太久没看背包，一时想不起来怎么memo。其实这个是背包1，取和不取求max。跟[L92 Backpack](l92-backpack.md)比较相似，只是这是二维的而已。T:O(l \* m \* n), filling out a 3d matrix, S:O(l \* m \* n) the memo 3d matrix

```java
public int findMaxForm(String[] strs, int m, int n) {
    if (strs == null || strs.length == 0) {
        return 0;
    }

    int[][] zerosAndOnes = zerosAndOnes(strs);
    int[][][] memo = new int[strs.length][m + 1][n + 1];
    return recurHelper(memo, zerosAndOnes, m, n, 0);
}

private int recurHelper(int[][][] memo, int[][] zerosAndOnes, int zeros, int ones, int start) {
    if (start >= zerosAndOnes.length) {
        return 0;
    }

    if (memo[start][zeros][ones] != 0) {
        return memo[start][zeros][ones];
    }

    int taken = -1;
    if (zeros - zerosAndOnes[start][0] >= 0 && ones - zerosAndOnes[start][1] >= 0) {
        taken = recurHelper(memo, zerosAndOnes, zeros - zerosAndOnes[start][0], ones - zerosAndOnes[start][1], start + 1) + 1;
    }
    int notTaken = recurHelper(memo, zerosAndOnes, zeros, ones, start + 1);

    memo[start][zeros][ones] = Math.max(taken, notTaken);
    return memo[start][zeros][ones];
}

private int[][] zerosAndOnes(String[] strs) {
    int[][] result = new int[strs.length][];
    for (int i = 0; i < strs.length; i++) {
        int[] zeroAndOne = new int[2];
        for (char c : strs[i].toCharArray()) {
            zeroAndOne[c - '0']++;
        }
        result[i] = zeroAndOne;
    }
    return result;
}
```
