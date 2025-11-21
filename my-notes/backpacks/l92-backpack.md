# L92 Backpack

Given\_n\_items with size Ai, an integer\_m\_denotes the size of a backpack. How full you can fill this backpack?

## Notice

You can not divide any item into small pieces.

**Example**

If we have`4`items with size`[2, 3, 5, 7]`, the backpack size is 11, we can select`[2, 3, 5]`, so that the max size we can fill this backpack is`10`. If the backpack size is`12`. we can select`[2, 3, 7]`so that we can fulfill the backpack.

You function should return the max size we can fill in the given backpack.

[**Challenge**](http://www.lintcode.com/en/problem/backpack/#challenge)

O(n x m) time and O(m) memory.

O(n x m) memory is also acceptable if you do not know how to optimize memory.

例子：

|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
| - | - | - | - | - | - | - | - | - | - | - | -- | -- |
| 0 | T | F | F | F | F | F | F | F | F | F | F  | F  |
| 2 | T | F | T | F | F | F | F | F | F | F | F  | F  |
| 3 | T | F | T | T | F | T | F | F | F | F | F  | F  |
| 5 | T | F | T | T | F | T | F | T | T | F | T  | F  |
| 7 | T | F | T | T | F | T | F | T | T | T | T  | F  |

```java
public int backPack(int m, int[] A) {
    if (m < 0 || A == null || A.length == 0) {
        return -1;
    }

    boolean[][] dp = new boolean[A.length + 1][m + 1];//dp[items number][max value we get]

    for (int i = 0; i <= A.length; i++) {
        dp[i][0] = true;
    }

    for (int i = 1; i <= A.length; i++) {
        for (int j = 1; j <= m; j++) {
            dp[i][j] = dp[i - 1][j];// not taking j item, status will be the same as before (i - 1) item
            if ((j >= A[i - 1]) && dp[i - 1][j - A[i - 1]]) {// taking item j
                dp[i][j] = true;
            }
        }
    }

    int result = m; //在最后一行找答案，从后面往前找第一个符合符合条件的，第一个为true的
    while (!dp[A.length][result]) {
        result--;
    }

    return result;
}
```
