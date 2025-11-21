# 526 Beautiful Arrangement

Suppose you have `n` integers labeled `1` through `n`. A permutation of those `n` integers `perm` (**1-indexed**) is considered a **beautiful arrangement** if for every `i` (`1 <= i <= n`), **either** of the following is true:

* `perm[i]` is divisible by `i`.
* `i` is divisible by `perm[i]`.

Given an integer `n`, return _the **number** of the **beautiful arrangements** that you can construct_.

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: 
The first beautiful arrangement is [1,2]:
    - perm[1] = 1 is divisible by i = 1
    - perm[2] = 2 is divisible by i = 2
The second beautiful arrangement is [2,1]:
    - perm[1] = 2 is divisible by i = 1
    - i = 2 is divisible by perm[2] = 1
```

**Example 2:**

```
Input: n = 1
Output: 1
```

**Constraints:**

* `1 <= n <= 15`

这题，一看，可以brute force，跟[L15 permutation](l15-permutation.md)差不多，就是进入下一层循环的条件多加了点，要整除下标或者被下标整除。但...看到求方案数，就心想，会不会是DP呢，想了半天，好像没啥结果。因为想了45min，所以就brute force直接上了。后来还想用前序优化，但其实在进入下一层梦境之前的判断已经是同样的效果。T:O(res), res为答案数目，S:O(n)，因为用了visited。

```java
int count = 0;
public int countArrangement(int n) {
    if (n <= 1) {
        return n;
    }

    boolean[] visited = new boolean[n + 1];
    recurHelper(n, visited, 0);

    return count;
}

private void recurHelper(int total, boolean[] visited, int index) {
    if (total == index) {
        count++;
        return;
    }

    for (int i = 1; i <= total; i++) {
        if (visited[i] == false)  {
            if (i % (index + 1) == 0 || (index + 1) % i == 0) {
                visited[i] = true;
                recurHelper(total, visited, index + 1);
                visited[i] = false;
            }
        }
    }
}
```
