# 452 Minimum Number of Arrows to Burst Ballons

There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104balloons.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstartand xendbursts by an arrow shot at x if xstart≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.

**Example:**

```
Input:[[10,16], [2,8], [1,6], [7,12]]
Output:2

Explanation:
One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) 
and another arrow at x = 11 (bursting the other two balloons).
```

这题有点难，看上去以为是line sweep，其实是贪心。首先还是得把那些interval（气球）按起点排序，如果起点相等按终点排，这里没排也过了OJ，有空再深入研究。排完以后，我们初始化res为1，因为球不为空，所以怎么也得射一箭。我们用end来记录结束位置。如果下一个球的开始位置小于等于这个end的话，证明这一箭能够把下一个气球也打了。打完记得更新end的位置（min（end，现在球的end））。比如球1跨度是【1，9】球2的跨度是【2，5】，球3的跨度是【6，7】，这时我们在打完第二个球的时候要把end更新为5，因为一箭打不完3个球。然后另一种情况就是，我们打不完要把res++表示多加一箭。在加了以后同样得更新现在这个end的位置，表示这一箭可以打到的宽度。不看答案想不出。因为要排序，所以T：O（NlogN), S: O（logN）

过了几年之后看了OJ的答案，发现OJ排的是end的坐标而不是start的。思路应该是一样的。

```java
public int findMinArrowShots(int[][] points) {
    if (points == null || points.length == 0 || points[0].length == 0) {
        return 0;
    }

    /*
    Arrays.sort(points, new Comparator<int[]>() {
        public int compare(int[] a, int[] b) {
            return a[0] - b[0];
        }
    });
    */
    // 过了几年之后，OJ加了overflow的数字，直接比较不用加减，就不会overflow了
    Arrays.sort(points, (a, b) -> {
        if (a[0] == b[0]) return 0;
        if (a[0] < b[0]) return -1;
        return 1;
    });

    int res = 1;
    int[] last = points[0];
    int end = last[1];
    for (int i = 1; i < points.length; i++) {
        int[] cur = points[i];
        if (end >= cur[0]) {
            end = Math.min(end, cur[1]);
        } else {
            res++;
            end = cur[1];
        }
    }
    return res;
}

// OJ的答案，上一份
public int findMinArrowShots(int[][] points) {
    if (points.length == 0) return 0;

    // sort by x_end
    Arrays.sort(points, (o1, o2) -> {
        // We can't simply use the o1[1] - o2[1] trick, as this will cause an 
        // integer overflow for very large or small values.
        if (o1[1] == o2[1]) return 0;
        if (o1[1] < o2[1]) return -1;
        return 1;
    });

    int arrows = 1;
    int xStart, xEnd, firstEnd = points[0][1];
    for (int[] p: points) {
        xStart = p[0];
        xEnd = p[1];
        // if the current balloon starts after the end of another one,
        // one needs one more arrow
        if (firstEnd < xStart) {
            arrows++;
            firstEnd = xEnd;
        }
    }

    return arrows;
}
```
