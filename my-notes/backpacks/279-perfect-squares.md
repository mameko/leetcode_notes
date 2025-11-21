# 279 Perfect Squares

Given a positive integern, find the least number of perfect square numbers (for example,`1, 4, 9, 16, ...`) which sum ton.

For example, given n=`12`, return`3`because`12 = 4 + 4 + 4`; given n=`13`, return`2`because`13 = 4 + 9`.

一眼看上去就像是背包，n就是背包容量，然后item就是那些平方数。每个item可以反复取，看装满容量的最小数目是什么。

```java
public int numSquares(int n) {
    if (n < 2) {
        return 1;
    }

    // find all the squres smaller than n
    ArrayList<Integer> squares = new ArrayList<>();
    int s = 1;
    while (s * s <= n) {
        squares.add(s * s);
        s++;
    }

    // backpack, item = all the squares, value = 0 to n
    int[][] dp = new int[squares.size() + 1][n + 1];

    // init first row to MAX, because you need to put infinite among of 0 item in to fill the bag
    for (int i = 0; i <= n; i++) {
        dp[0][i] = Integer.MAX_VALUE;
    }

    for (int i = 1; i <= squares.size(); i++) {
        for (int j = 1; j <= n; j++) {
            dp[i][j] = dp[i - 1][j];
            if (j >= squares.get(i - 1)) {
                dp[i][j] = Math.min(dp[i][j], dp[i][j - squares.get(i - 1)] + 1);
            }
        }
    }

    return dp[squares.size()][n];
}
```

来个BFS的。具体做法还是先找squares数组，把邻居都放进去。图的节点是还剩下多少，返回条件是如果剩下0了，我们就知道已经找到了。写法不是很复杂，但时间复杂度很难算，因为其实枚举了所有，但中间可以通过判断减完以后是否负数来剪枝。

```java
public int numSquares(int n) {
    if (n < 0) {
        return -1;
    }

    List<Integer> squares = new ArrayList<>();
    int s = 1;
    while (s * s <= n) {
        squares.add(s * s);
        s++;
    }

    Queue<Integer> queue = new LinkedList<>();
    queue.offer(n);
    int step = 0;

    while (!queue.isEmpty()) {
        step++;
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int cur = queue.poll();

            for (int j = 0; j < squares.size(); j++) {
                int nei = squares.get(j);
                int diff = cur - nei;
                if (diff == 0) {
                    return step;
                } else if (diff > 0) {
                    queue.offer(diff);
                } else {
                    break;
                }
            }
        }
    }

    return -1;
}
```
