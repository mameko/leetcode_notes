# 302 Smallest Rectangle Enclosing Black Pixels

An image is represented by a binary matrix with`0`as a white pixel and`1`as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location`(x, y)`of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

For example, given the following image:

```
[
  "0010",
  "0110",
  "0100"
]
```

and`x = 0`,`y = 2`,

Return`6`.

这题有3种方法，

第一种：暴力，O(n^2)

```
public int minArea(char[][] image, int x, int y) {
    if (image == null || image.length == 0 || image[0].length == 0) {
        return 0;
    }

    int n = image.length;
    int m = image[0].length;

    int up = n;
    int left = m;
    int down = 0;
    int right = 0;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (image[i][j] == '1') {
                up = Math.min(up, i);
                down = Math.max(down, i);
                left = Math.min(left, j);
                right = Math.max(right, j);
            }
        }
    }

    return (down - up + 1) * (right - left + 1);
}
```

第二种bfs：O(黑点数目)，所以worst case还是O(n^2)

```java
public int minArea(char[][] image, int x, int y) {    
    public int minArea(char[][] image, int x, int y) {
        if (image == null || image.length == 0 || image[0].length == 0) {
            return 0;
        }

        int n = image.length;
        int m = image[0].length;

        int xmax = 0;
        int xmin = n;
        int ymax = 0;
        int ymin = m;

        // bfs to find the black points
        Queue<Pair> queue = new LinkedList<>();

        Pair start = new Pair(x, y);
        queue.offer(start);
        image[x][y] ='0';

        int[] xdir = {0, 0, 1, -1};
        int[] ydir = {1, -1, 0, 0};

        while (!queue.isEmpty()) {
            Pair cur = queue.poll();

            xmax = Math.max(xmax, cur.x);
            xmin = Math.min(xmin, cur.x);
            ymax = Math.max(ymax, cur.y);
            ymin = Math.min(ymin, cur.y);


            for (int k = 0; k < 4; k++) {
                int nexti = cur.x + xdir[k];
                int nextj = cur.y + ydir[k];

                if (inBound(nexti, nextj, image) && image[nexti][nextj] == '1') {
                    image[nexti][nextj] = '0';
                    Pair newPair = new Pair(nexti, nextj);
                    queue.offer(newPair);
                }
            }
        }


        // calculate area
        int w = ymax - ymin + 1;
        int h = xmax - xmin + 1;
        return w * h;
    }

    private boolean inBound(int row, int col, char[][] image) {
        if (row < 0 || col < 0 || row >= image.length || col >= image[0].length) {
            return false;
        }

        return true;
    }
}

class Pair {
    int x;
    int y;

    public Pair(int i, int j) {
        x = i;
        y = j;
    }
}
```

第三种：2分找最外围的4个点，O(logm + logn) ?

```java
public int minArea(char[][] image, int x, int y) {
    if (image == null || image.length == 0 || image[0].length == 0) {
        return 0;
    }

    int n = image.length;
    int m = image[0].length;

    // binary search on bounds

    // find top, [0, x]
    int top = x;
    int s1 = 0;
    int e1 = x;
    while (s1 + 1 < e1) {
        int mid = s1 + (e1 - s1) / 2;
        if (rowhas1(mid, image)) {
            e1 = mid;
        } else {
            s1 = mid;
        }
    }

    if (rowhas1(s1, image)) {
        top = s1;
    } else {
        top = e1;
    }

    // find bottom, [x, n - 1]
    int bottom = x;
    int s2 = x;
    int e2 = n - 1;
    while (s2 + 1 < e2) {
        int mid = s2 + (e2 - s2) / 2;
        if (rowhas1(mid, image)) {
            s2 = mid;
        } else {
            e2 = mid;
        }
    }

    if (rowhas1(e2, image)) {
        bottom = e2;
    } else {
        bottom = s2;
    }

    // find left, [0, y]
    int left = y;
    int s3 = 0;
    int e3 = y;
    while (s3 + 1 < e3) {
        int mid = s3 + (e3 - s3) / 2;
        if (colhas1(mid, image)) {
            e3 = mid;
        } else {
            s3 = mid;
        }
    }

    if (colhas1(s3, image)) {
        left = s3;
    } else {
        left = e3;
    }

    // find right, [y, m - 1]
    int right = y;
    int s4 = y;
    int e4 = m - 1;
    while (s4 + 1 < e4) {
        int mid = s4 + (e4 - s4) / 2;
        if (colhas1(mid, image)) {
            s4 = mid;
        } else {
            e4 = mid;
        }
    }

    if (colhas1(e4, image)) {
        right = e4;
    } else {
        right = s4;
    }

    return (right - left + 1) * (bottom - top + 1);
}

    private boolean colhas1(int j, char[][] image) {
        for (int i = 0; i < image.length; i++) {
            if (image[i][j] == '1') {
                return true;
            }
        }

        return false;
    }

    private boolean rowhas1(int i, char[][] image) {
        for (int j = 0; j < image[0].length; j++) {
            if (image[i][j] == '1') {
                return true;
            }
        }

        return false;
    }
```
