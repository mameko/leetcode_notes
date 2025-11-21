# 42 Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example,\
Given`[0,1,0,2,1,0,1,3,2,1,2,1]`, return`6`.

![](<../../.gitbook/assets/image (4).png>)

The above elevation map is represented by array \[0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

这题有两种做法，首先可以用DP。左往右来一遍，右往左来一遍。最后加起来，有点像Candy那些，留意left right traversal收藏夹。

```java
public int trapRainWater(int[] heights) {
    if (heights == null || heights.length == 0) {
        return 0;
    }

    int[] left = new int[heights.length];
    int[] right = new int[heights.length];

    int leftM = heights[0];
    int rightM = heights[heights.length - 1];

    for (int i = 0; i < heights.length; i++) {
        left[i] = Math.max(leftM, heights[i]);
        leftM = Math.max(leftM, heights[i]);
    }

    for (int i = heights.length - 1; i >= 0; i--) {
        right[i] = Math.max(rightM, heights[i]);
        rightM = Math.max(rightM, heights[i]);
    }

    int res = 0;
    for (int i = 0; i < heights.length; i++) {
        res += Math.min(left[i], right[i]) - heights[i];
    }

    return res;
}
```

下面是两个指针的做法，其实好像是把dp的空间优化了一样，从数组变成了两个变量。用level来记录现在的max height， 再把max height和现在两边高度中小的那个相减算出差值 这种做法在。每次加到res里。L364 Trapping rain water II上也用到。

```java
public int trap(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }

    // use level to track current max height.
    int level = 0;
    int left = 0;
    int right = height.length - 1;
    int res = 0;

    while (left < right) {
        int h = Math.min(height[left], height[right]);
        level = Math.max(h, level);
        res += level - h;     

        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return res;
}
```
