# 407 Trapping Rain Water II

Given an `m x n` matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.

**Example:**

```
Given the following 3x6 height map:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]

Return 4.
```

![](https://assets.leetcode.com/uploads/2018/10/13/rainwater_empty.png)

The above image represents the elevation map `[[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]` before the rain.

![](https://assets.leetcode.com/uploads/2018/10/13/rainwater_fill.png)

After the rain, water is trapped between the blocks. The total volume of water trapped is 4.

**Constraints:**

* `1 <= m, n <= 110`
* `0 <= heightMap[i][j] <= 20000`

这一题的思路跟[42 Trapping Rain Water](../two-pointers-other/42-trapping-rain-water.md)相似，主要是找围着某一点的柱子里，最矮的那一个。然后能装多少水，就是那个跟被围着的点的差。我们还得边检查，边更新现在围住的“边”的最低高度。这一题因为要在一串数字里（围着的边）不断地找最小值，所以用了堆

