# 326 Power of Three

Given an integer `n`, return _`true` if it is a power of three. Otherwise, return `false`_.

An integer `n` is a power of three, if there exists an integer `x` such that `n == 3^x`.

&#x20;

**Example 1:**

<pre><code>Input: n = 27
<strong>Output: true
</strong><strong>Explanation: 27 = 3^3
</strong></code></pre>

**Example 2:**

<pre><code>Input: n = 0
<strong>Output: false
</strong><strong>Explanation: There is no x where 3^x = 0.
</strong></code></pre>

**Example 3:**

<pre><code>Input: n = -1
<strong>Output: false
</strong><strong>Explanation: There is no x where 3^x = (-1).
</strong></code></pre>

&#x20;**Constraints:**

* `-231 <= n <= 231 - 1`

&#x20;**Follow up:** Could you solve it without loops/recursion?

这题不难，用循环直接除就是了。因为3的n次方的数，只能有3这个因数。如果除出来的数字不是1的话，那么就不是3的次方了。譬如，27 = 3 \* 3 \* 3。但12 = 3 \* 4。这里，T：O(log3N), S: O(1）。这里follow up的解法是数学解法。因为n的范围是正整数，然后3是质数。在正整数范围内最大的3的n次方是19。所以如果n能被3的19次方整除，那么n就是3的power。

```java
public boolean isPowerOfThree(int n) {
    if (n <= 0) {
        return false;
    }
    
    if (n == 1) {
        return true;
    }
    
    while (n >= 3) {
        if (n % 3 != 0) {
            return false;
        }
        n = n / 3;
    }
    
    return n == 1;
}

// 下面是leetcode答案
public boolean isPowerOfThree(int n) {
    if (n < 1) {
        return false;
    }

    while (n % 3 == 0) {
        n /= 3;
    }

    return n == 1;
}

// 数学解法，T:O(1), S:O(1)
public boolean isPowerOfThree(int n) {
    return n > 0 && 1162261467 % n == 0;
}
}j
```
