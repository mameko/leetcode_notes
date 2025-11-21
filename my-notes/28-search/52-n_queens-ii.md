# 52 N\_Queens II

ollow up for N-Queens problem.

Now, instead outputting board configurations, return the total number of distinct solutions.

**Example**

For n=4, there are 2 distinct solutions.

这题虽然求方案数，但还是得搜，不能DP.

```java
    public int totalNQueens(int n) {
        if (n < 1) {
            return 0;
        }
        ArrayList<Pair> curRes = new ArrayList<>();
        int[] res = new int[1];

        NQueenHelper(n, curRes, 0, res);

        return res[0];
    }

    private void NQueenHelper(int n, ArrayList<Pair> curRes, int level, int[] res) {
        if (level == n) {
            res[0]++;
            return;
        }


        for (int j = 0; j < n; j++) {
            if (isValid(level, j, curRes)) {
                Pair tmp = new Pair(level, j);
                curRes.add(tmp);
                NQueenHelper(n, curRes, level + 1, res);
                curRes.remove(curRes.size() - 1);
            }
        }

    }


    private boolean isValid(int i, int j, ArrayList<Pair> locatedQueens) {
        for (Pair p : locatedQueens) {
            if (p.i == i || p.j == j || (p.i - p.j) == (i - j) || (p.i + p.j) == (i + j)) {
                return false;
            }
        }
        return true;
    }
};

class Pair {
    int i;
    int j;

    public Pair(int i, int j) {
        this.i = i;
        this.j = j;
    }
}
```
