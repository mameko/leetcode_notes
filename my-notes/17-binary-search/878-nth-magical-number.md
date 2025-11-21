# 878 Nth Magical Number

A positive integer is _magical_ if it is divisible by either `a` or `b`.

Given the three integers `n`, `a`, and `b`, return the `nth` magical number. Since the answer may be very large, **return it modulo** `10^9 + 7`.&#x20;

**Example 1:**

<pre><code>Input: n = 1, a = 2, b = 3
<strong>Output: 2
</strong></code></pre>

**Example 2:**

<pre><code>Input: n = 4, a = 2, b = 3
<strong>Output: 6
</strong></code></pre>

&#x20;**Constraints:**

* `1 <= n <= 10^9`
* `2 <= a, b <= 4 * 10^4`

这题其实有数学解法的，但是觉得自己看不懂，所以没看。一开始看以为这一题像之前有些dp的题那样，用heap啥的从小到大一个个找。但是感觉复杂的很高，想不通，最后还是看了答案。这题首先需要发现些规律，譬如，n = 17，A = 2, B = 3, 那么在\[1, 17]里有多少个可以被A,2整除的数字呢？答案是17/2 = 8（2，4，6，8，10，12，14，16），那么有多少个可以被B，3整除的呢？17/3 = 5（3，6，9，12，15）。但在\[1, 17]里能被2or3整除的数其实只有11个（2，3，4，6，8，9，10，12，14，15，16），不是5+8=13。这是因为6，12我们重复算了，这里要减去。然后这个6和12其实是A，B的公倍数。这里同理，我们可以算出\[1,17]里有 17/最小公倍数 个数字，17/6 = 2(6, 12)。所以我们知道，我们要求的是一个x，x满足这个条件：x/A + x/B - x/lcm。这里我们用2分来找第一个满足这个条件的数字。

那么二分的上下边界是什么呢？start可以取min(A, B)因为n的range从1开始，end可以去n\*min（A,B)因为第n个，worst case，我们中间啥都没有。因为二分的值在start和end之间，所以可以套九章的模板。这里注意return的时候要mod大质数。

```java
int M = 1_000_000_007;

public int nthMagicalNumber(int n, int a, int b) {
    if (a < 2 || b < 2 || n < 1) {
        return 0;
    }
    
    int leastCommonMultiple = a / gcd(a, b) * b;
    long start = 2l;
    long end = (long) n * Math.min(a, b);
    while (start + 1 < end) {
        long mid = start + (end - start) / 2;
        if (isNth(mid, a, b, leastCommonMultiple) < n) {
            start = mid;
        } else if (isNth(mid, a, b, leastCommonMultiple) > n) {
            end = mid;
        } 
        else {
//                return (int)(mid % M); //一开始直接return，不行，要找第一个满足条件的
        	end = mid;
        }
    }
    
    if (isNth(start, a, b, leastCommonMultiple) == n) {
        return (int) (start % M);
    } else {
        return (int) (end % M);
    }
}

private long isNth(long mid, int a, int b, int leastCommonMultiple) {
    return mid / a + mid / b - mid / leastCommonMultiple;
}

private int gcd(int a, int b) {
    while (b != 0) {
        int tmp = b;
        b = a % b;
        a = tmp;
    }

    return a;
java}
```
