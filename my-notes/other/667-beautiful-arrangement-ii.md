# 667 Beautiful Arrangement II

Given two integers `n` and `k`, you need to construct a list which contains `n` different positive integers ranging from `1` to `n` and obeys the following requirement:\
Suppose this list is \[a1, a2, a3, ... , an], then the list \[|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|] has exactly `k` distinct integers.

If there are multiple answers, print any of them.

**Example 1:**

```
Input: n = 3, k = 1
Output: [1, 2, 3]
Explanation: The [1, 2, 3] has three different positive integers ranging from 1 to 3, and the [1, 1] has exactly 1 distinct integer: 1.
```

**Example 2:**

```
Input: n = 3, k = 2
Output: [1, 3, 2]
Explanation: The [1, 3, 2] has three different positive integers ranging from 1 to 3, and the [2, 1] has exactly 2 distinct integers: 1 and 2.
```

**Note:**

1. The `n` and `k` are in the range 1 <= k < n <= 104.

其实跟[526 Beautiful Arrangement](../3-left-right-tranvesal-max-array-stocks/526-beautiful-arrangement.md)毫无瓜葛。我一开始还brute force了，结果不出所料直接TLE了。然后想了半天没想出来怎么弄，就只能看答案了。其实这题是要找规律，直接生成数字。因为要判断差，所以brute force不能优化，要一直递归到底找到答案为止。

其实，这一题是可以这样生成：譬如，n = 6, k = 3, 我们有\[1, | 2, 3, 4, 5, 6]，我们可以把\[2, 3, 4, 5, 6]翻转，得到一个diff。\[1, 6, | 5, 4, 3, 2]，然后再翻转，得到第二个diff，\[1, 6, 2, 3, 4, 5]，如此类推，翻转k次以后就得到k个diff了。但这个方法要翻转很多次，所以可能要O（n2）

这里规律是，从\[1，n]的数组，最多可以生成的k个diff是\[n - 1]，\[1, n, 2, n-1, 3, n-2, ....].最少的k是1.\[1, 2, 3, ..., n]。知道这个规律以后，我们就可以把答案分成两段，一段是k-1的升序片段，另一段能产生最大diff的n - k。譬如，n = 6, k = 3, 我们有\[1, 2, 3, 4, 5, 6]，那么我们就可以生成\[1, 2, | 3, 6, 4, 5]或者\[1, 6, | 2, 3, 4, 5]。具体做法是，当k是基数时，从左边取数，当k时偶数时，从右边开始取数（可能是因为这是递增序列）。T:O(n)

```java
public int[] constructArray(int n, int k) {
    int[] res = new int[n];
    if (n < 1 || k < 1) {
        return res;
    }

    int i = 1;
    int j = n;
    int index = 0;
    while (i <= j) { // 判断何时停止加数
        if (k > 1) {//最后的一个diff是递增序列的,所以k - 1是条件
            if (k % 2 == 1) {//基数从头开始取
                res[index++] = i++;    
            } else {//偶数从尾巴开始取
                res[index++] = j--;
            }
            k--;//每次产生一个diff，k要减一
        } else {
            res[index++] = i++;//最后的diff是递增序列产生的
        }
    }

    return res;
}
```
