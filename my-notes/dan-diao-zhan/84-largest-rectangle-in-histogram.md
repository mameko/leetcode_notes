# 84 Largest Rectangle in Histogram

Given\_n\_non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

![](<../../.gitbook/assets/image (10).png>)

Above is a histogram where width of each bar is 1, given height =`[2,1,5,6,2,3]`.

![](<../../.gitbook/assets/image (9).png>)

The largest rectangle is shown in the shaded area, which has area =`10`unit.

**Example**

Given height = `[2,1,5,6,2,3]`,\
return `10`.

这题暴力的解法是O(n^2)的，所以虽然求最大值但不是DP。这里用一个单调栈可以把复杂度降低到O(n)。因为每次我们算面积时，我们需要知道这个高度的左右两边第一个比它小的柱子的高度。左边，就是左边第一个比它小的，右边第一个比它小的，是把它pop出来的那个数。所以我们用单调栈。而且栈里存的不是栈的高度，而是下标。这样比较方便取得跨度。然后通过下标我们也很容易得到高度。最后加的那个-1，是要把栈里所有的东西pop出来。

```java
public int largestRectangleArea(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }

    Stack<Integer> stack = new Stack<>();
    int i = 0;
    int max = Integer.MIN_VALUE;
    while (i <= height.length) {
        int cur = (i == height.length) ? -1: height[i];
        while (!stack.isEmpty() && cur <= height[stack.peek()]) {
            int h = height[stack.pop()];
            int w = stack.isEmpty() ? i : i - stack.peek() - 1;
            int area = h * w;
            max = Math.max(max, area);
        }

        stack.push(i);
        i++;
    }

    return max;
}
```
