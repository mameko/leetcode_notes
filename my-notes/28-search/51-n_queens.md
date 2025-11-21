# 51 N\_Queens

The n-queens puzzle is the problem of placing n queens on an`n×n`chessboard such that no two queens attack each other.

Given an integer`n`, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where`'Q'`and`'.'`both indicate a queen and an empty space respectively.

**Example**

There exist two distinct solutions to the 4-queens puzzle:

```
[
  // Solution 1
  [".Q..",
   "...Q",
   "Q...",
   "..Q."
  ],
  // Solution 2
  ["..Q.",
   "Q...",
   "...Q",
   ".Q.."
  ]
]
```

[**Challenge**](http://www.lintcode.com/en/problem/n-queens/#challenge)

Can you do it without recursion?

```java
   ArrayList<ArrayList<String>> solveNQueens(int n) {
        ArrayList<ArrayList<String>> finalRes = new ArrayList<>();
        if (n < 1) {
            return finalRes;
        }

        ArrayList<ArrayList<Pair>> res = new ArrayList<>();
        ArrayList<Pair> tmp = new ArrayList<>();

        placeQueen(res, tmp, 0, n);

        translateRes(res, finalRes, n);

        return finalRes;
    }

    private void placeQueen(ArrayList<ArrayList<Pair>> res, ArrayList<Pair> tmp, int row, int n) {
        if (row == n) {
            res.add(new ArrayList(tmp));
            return;
        }

        for (int i = 0; i < n; i++) {
            if (isValid(row, i, tmp)) {
                Pair p = new Pair(row, i);
                tmp.add(p);
                placeQueen(res, tmp, row + 1, n);
                tmp.remove(tmp.size() - 1);
            }
        }
    }

    private boolean isValid(int i, int j, ArrayList<Pair> queens) {
        for (Pair p : queens) {
            if (i == p.i || j == p.j || ((p.i - p.j == (i - j)) || ((p.i + p.j) == (i + j)))) {
                return false;
            }
        }
        return true;
    }

    private void translateRes(ArrayList<ArrayList<Pair>> res, ArrayList<ArrayList<String>> finalR, int n) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            sb.append(".");
        }

        ArrayList<String> base = new ArrayList<String>();
        for (int i = 0; i < n; i++) {
            base.add(sb.toString());
        }

        for (ArrayList<Pair> singleAns : res) {
            ArrayList<String> singleFin = new ArrayList<>(base);

            for (Pair p : singleAns) {
                String tmp = singleFin.get(p.i);
                char[] tmpchar = tmp.toCharArray();
                tmpchar[p.j] = 'Q';
                String withQ = new String(tmpchar);
                singleFin.set(p.i, withQ);
            }

            finalR.add(singleFin);
        }
    }
};

class Pair {
    int i;
    int j;

    public Pair(int iLoc, int jLoc) {
        i = iLoc;
        j = jLoc;
    }
}
```
