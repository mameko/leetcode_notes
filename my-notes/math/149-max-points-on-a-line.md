# 149 Max Points on a Line

Given\_n\_points on a 2D plane, find the maximum number of points that lie on the same straight line.

**Example 1:**

```
Input:
 [[1,1],[2,2],[3,3]]

Output:
 3

Explanation:

^
|
|        o
|     o
|  o  
+-------------
>

0  1  2  3  4
```

**Example 2:**

```
Input:
 [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]

Output:
 4

Explanation:

^
|
|  o
|     o        o
|        o
|  o        o
+-------------------
>

0  1  2  3  4  5  6
```

这题好难，本来想N方地做， 把相同的点存在到map里，key是k和interception，然后value是一set放着所有的点。然后各种不行。

然后只好抄了大神的解法做参考：N^3叉积法[https://yiminghe.iteye.com/blog/568666](https://yiminghe.iteye.com/blog/568666)

```java
public int maxPoints(Point[] points) {
    int res = 0, n = points.length;
    for (int i = 0; i < n; ++i) {
        int duplicate = 1;
        for (int j = i + 1; j < n; ++j) {
            int cnt = 0;
            long x1 = points[i].x, y1 = points[i].y;
            long x2 = points[j].x, y2 = points[j].y;
            if (x1 == x2 && y1 == y2) {++duplicate;continue;}
            for (int k = 0; k < n; ++k) {
                int x3 = points[k].x, y3 = points[k].y;
                if (x1*y2 + x2*y3 + x3*y1 - x3*y2 - x2*y1 - x1 * y3 == 0) { //判断两矢量共线
                    ++cnt;
                }
            }
            res = Math.max(res, cnt);
        }
        res = Math.max(res, duplicate);
    }
    return res;
}
```

再加一个用gcd的...不明觉厉：

```java
/*
 *  A line is determined by two factors,say y=ax+b
 *
 *  If two points(x1,y1) (x2,y2) are on the same line(Of course).

 *  Consider the gap between two points.

 *  We have (y2-y1)=a(x2-x1),a=(y2-y1)/(x2-x1) a is a rational, b is canceled since b is a constant

 *  If a third point (x3,y3) are on the same line. So we must have y3=ax3+b

 *  Thus,(y3-y1)/(x3-x1)=(y2-y1)/(x2-x1)=a

 *  Since a is a rational, there exists y0 and x0, y0/x0=(y3-y1)/(x3-x1)=(y2-y1)/(x2-x1)=a

 *  So we can use y0&x0 to track a line;
 */
    public int maxPoints(Point[] points) {
        if (points == null) return 0;

        int solution = 0;

        for (int i = 0; i < points.length; i++)
        {
            Map<String, Integer> map = new HashMap<>();
            int duplicate = 0;
            int max = 0;
            for (int j = i + 1; j < points.length; j++)
            {
                int deltaX = points[j].x - points[i].x;
                int deltaY = points[j].y - points[i].y;

                if (deltaX == 0 && deltaY == 0)
                {
                    duplicate++;
                    continue;
                }

                int gcd = gcd(deltaX, deltaY);
                int dX = deltaX / gcd;
                int dY = deltaY / gcd;

                map.put(dX + "," + dY, map.getOrDefault(dX + "," + dY, 0) + 1);
                max = Math.max(max, map.get(dX + "," + dY));
            }

            solution = Math.max(solution, max + duplicate + 1);
        }

        return solution;
    }

    public int gcd(int a, int b)
    {
        if (b == 0)
            return a;
        return gcd(b, a%b);
    }
```
