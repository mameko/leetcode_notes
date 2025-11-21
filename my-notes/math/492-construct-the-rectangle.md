# 492 Construct the Rectangle

A web developer needs to know how to design a web page's size. So, given a specific rectangular web page’s area, your job by now is to design a rectangular web page, whose length L and width W satisfy the following requirements:

1. The area of the rectangular web page you designed must equal to the given target area.
2. The width `W` should not be larger than the length `L`, which means `L >= W`.
3. The difference between length `L` and width `W` should be as small as possible.

Return _an array `[L, W]` where `L` and `W` are the length and width of the web page you designed in sequence._

&#x20;

**Example 1:**

<pre><code>Input: area = 4
<strong>Output: [2,2]
</strong><strong>Explanation: The target area is 4, and all the possible ways to construct it are 
</strong><strong>[1,4], [2,2], [4,1]. 
</strong>But according to requirement 2, [1,4] is illegal; 
according to requirement 3,  [4,1] is not optimal compared to [2,2]. 
So the length L is 2, and the width W is 2.
</code></pre>

**Example 2:**

<pre><code>Input: area = 37
<strong>Output: [37,1]
</strong></code></pre>

**Example 3:**

<pre><code>Input: area = 122122
<strong>Output: [427,286]
</strong></code></pre>

&#x20;**Constraints:**

* `1 <= area <= 10^7`

这题想复杂了，一眼看上去是不是二分呀，从一半开始，找到area那么大的数里，最小的能被整除的数。因为靠近中间diff才小。然而，二分写不出来，因为判断的条件好像不能一下砍一半。后来又想了想，是不是two pointer，从大小两边往中间找。但又不太像，因为不能两边同时移动。而且没有判断到底该移哪一边的条件。后来看了提示，还是没想出来。最后只好看答案了。其实，这里的范围因该是1到 sqrt（area），这是width的范围。没有什么更快的方式，只能每次减一来找。

```java
public int[] constructRectangle(int area) {
    if (area < 1) {
        return null;
    }
    
    int maxWidth = (int) Math.sqrt(area);
    
    while (area % maxWidth != 0) {
        maxWidth = maxWidth - 1;
    }

    int[] result = new int[2];
    result[0] = area / maxWidth;
    result[1] = maxWidth;
    
    return result;
}

// 下面补一个c++的，逆向思维，用乘法。
// area就夹在两个平方之间，然后把小于超出这个area之前的能被整除的数一边找，一遍记录下来。
// 这个被记录下来的数是最接近sqrt(area)（最大的width）
class Solution {
public:
    vector<int> constructRectangle(int area) {
        int r = 1;
        for (int i = 1; i * i <= area; ++i) {
            if (area % i == 0) r = i;
        }
        return {area / r, r};
    }
};
```
