# 528 Random Pick with Weight



You are given a **0-indexed** array of positive integers `w` where `w[i]` describes the **weight** of the `ith` index.

You need to implement the function `pickIndex()`, which **randomly** picks an index in the range `[0, w.length - 1]` (**inclusive**) and returns it. The **probability** of picking an index `i` is `w[i] / sum(w)`.

* For example, if `w = [1, 3]`, the probability of picking index `0` is `1 / (1 + 3) = 0.25` (i.e., `25%`), and the probability of picking index `1` is `3 / (1 + 3) = 0.75` (i.e., `75%`).

&#x20;

**Example 1:**

<pre><code>Input
["Solution","pickIndex"]
[[[1]],[]]
<strong>Output
</strong>[null,0]
<strong>Explanation
</strong>Solution solution = new Solution([1]);
solution.pickIndex(); // return 0. The only option is to return 0 since there is only one element in w.
</code></pre>

**Example 2:**

<pre><code>Input
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
<strong>Output
</strong>[null,1,1,1,1,0]
<strong>Explanation
</strong>Solution solution = new Solution([1, 3]);
solution.pickIndex(); // return 1. It is returning the second element (index = 1) that has a probability of 3/4.
solution.pickIndex(); // return 1
solution.pickIndex(); // return 1
solution.pickIndex(); // return 1
solution.pickIndex(); // return 0. It is returning the first element (index = 0) that has a probability of 1/4.

Since this is a randomization problem, multiple answers are allowed.
All of the following outputs can be considered correct:
[null,1,1,1,1,0]
[null,1,1,1,1,1]
[null,1,1,1,0,0]
[null,1,1,1,0,1]
[null,1,0,1,0,0]
......
and so on.
</code></pre>

&#x20;

**Constraints:**

* `1 <= w.length <= 104`
* `1 <= w[i] <= 105`
* `pickIndex` will be called at most `104` times.

这题，看了半天不知道要干嘛。后来看了解释，原来是要按weight random pick。就是，如果我们用random函数来random一堆数的话，每一个都被pick的概率是一样的。但现在我们要按weight来pick。譬如，\[1, 3]，那么1被pick的概率是25%，3是75%。那么怎么才能运用random来pick呢，做法是，把这个区间分成4份，譬如，java里random()会generate一个\[0.0, 1.0)之间的double，把它乘以4，那么就会落在\[0.0, 4.0)这个区间内。如果这个生成的数字落在\[0.0, 1.0)内，那么返回位置0。如果落在\[1.0, 4.0)里，那么返回位置1。再举一个例子\[1, 3, 2]，生成prefix sum \[1, 4, 6]，用random乘以max（6），那么如果生成的数字落在\[4, 6)之间，那么返回下标2。这题其实是求binary search prefixsum第一个比这个randomXmax大的位置。

这题T:O(N)，因为我们prefix的时候过了一遍，下面二分用了T：O(logN)，S：O（N）存了prefixsum数组。

这题的一个变种是，如果原数组老是变动的话，怎么解决，那么这就要用segment tree or BIT了

```java
    private int[] prefixSums;

    public Solution(int[] w) {
        this.prefixSums = new int[w.length];

        int prefixSum = 0;
        for (int i = 0; i < w.length; ++i) {
            prefixSum += w[i];
            this.prefixSums[i] = prefixSum;
        }
    }
    
    public int pickIndex() {        
        int max = prefixSums[prefixSums.length - 1];
        double target = max * Math.random();
        
        int start = 0;
        int end = prefixSums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (prefixSums[mid] == target) {
                end = mid;
            } else if (prefixSums[mid] < target) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        if (prefixSums[start] > target) {
            return start;
        } else {
            return end;
        }
    }
```
