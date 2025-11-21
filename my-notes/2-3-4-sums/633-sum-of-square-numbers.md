# 633 Sum of Square Numbers

Given a non-negative integer `c`, decide whether there're two integers `a` and `b` such that `a2 + b2 = c`.

&#x20;

**Example 1:**

<pre><code><strong>Input: c = 5
</strong><strong>Output: true
</strong><strong>Explanation: 1 * 1 + 2 * 2 = 5
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: c = 3
</strong><strong>Output: false
</strong></code></pre>

&#x20;

**Constraints:**

* `0 <= c <= 2^31 - 1`

`这题，一开始以为两个数a和b要不同的，所以很多例子没过，譬如4，8之类的。然后，看到是two prt，我就想能不能，直接生成一条list，存所有的squre之后的数字，然后一左一右地2ptr。后来大概100的时候卡住了，memory exceed limit。后来，想了半天只好看答案了。solution给了大部分都是枚举a，然后二分求b = c - a^2。这样可以T：O（sqrt(c)*logc)。后来，在问答区看到了大佬指出了2ptr的真正打开方式。原来不用真的生成一条list。只要找到left和right，每次直接++，--即可。`

```java
public boolean judgeSquareSum(int c) {
    if (c < 0) {
        return true;
    }

    long left = 0;
    // 譬如，sqrt（8）=2*根号2，floor一下大概是4，4 + 4 = 8，所以有边界是4
    // 这里还要注意全都得long，不然大数据过不了
    long right = (int)Math.floor(Math.sqrt(c));
    while (left <= right) {
        long sum = left * left + right * right;
        if (sum == c) {
            return true;
        } else if (sum < c) {
            left++;
        } else {
            right--;
        }
    }

    return false;
}  
```
