# 1037 Valid Boomerang



A _boomerang_ is a set of 3 points that are all distinct and **not** in a straight line.

Given a list of three points in the plane, return whether these points are a boomerang.

**Example 1:**

```
Input: [[1,1],[2,3],[3,2]]
Output: true
```

**Example 2:**

```
Input: [[1,1],[2,2],[3,3]]
Output: false
```

**Note:**

1. `points.length == 3`
2. `points[i].length == 2`
3. `0 <= points[i][j] <= 100`

这题考的是三点共线用向量的求法。当然我们可以用y = xk + b解方程，但实现起来很多边界条件，譬如k不存在之类的，所有很难控制。具体证明看这里[https://www.youtube.com/watch?v=L3EnbHgSUKg](https://www.youtube.com/watch?v=L3EnbHgSUKg)。主要就是求这个OA向量和OB向量的积：

![](<../../.gitbook/assets/image (20).png>)

这里O(x1, x2), A (x1, y1), B (x2, y2)

![](<../../.gitbook/assets/image (13).png>)

这里行列式我们会有正负号，但面积是没有的，所以要加绝对值。其实我们求面积其实是菱形的，要乘个1/2才是三角形的面积。如果面积不等于0，我们就知道三点能成为三角形，所以不共线。

```java
public boolean isBoomerang(int[][] points) {
    if (points == null || points.length == 0 || points[0].length == 0) {
        return false;
    }

    int x1 = points[0][0];
    int y1 = points[0][1];

    int x2 = points[1][0];
    int y2 = points[1][1];

    int x3 = points[2][0];
    int y3 = points[2][1];

    int x2_x1 = x2 - x1;
    int y2_y1 = y2 - y1;
    int x3_x1 = x3 - x1;
    int y3_y1 = y3 - y1;

    int area = x2_x1 * y3_y1 - x3_x1 * y2_y1;

    return Math.abs(area) != 0;        
}
```
