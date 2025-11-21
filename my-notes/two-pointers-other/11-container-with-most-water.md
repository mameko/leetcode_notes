# 11 Container With Most Water

Given n non-negative integers a1,a2, ...,an, where each represents a point at coordinate (i,ai).nvertical lines are drawn such that the two endpoints of lineiis at (i,ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

通过两个指针从两边开始往中间靠，每次算一个容量，边算边更新最大值。注意width是right - left，不用加一。

```java
public int maxArea(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }

    int left = 0;
    int right = height.length - 1;
    int max = 0;
    while (left < right) {
        int w = right - left;
        int h = Math.min(height[left], height[right]);
        int area = h * w;
        max = Math.max(area, max);

        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }

    return max;
}
```
