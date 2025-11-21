# 96 Unique Binary Search Tree

Givenn, how many structurally unique**BST's**(binary search trees) that store values 1...n?

For example,\
Givenn= 3, there are a total of 5 unique BST's.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

这题很高难度，主要是控制的时候比较难。这题要求的是数目，然后根据2叉树的特性我们可以得出下面的结果：

0 的时候有1棵树，因为空树也是树。dp\[0] = 1;

1 的时候有1棵树，因为左右都为0，只有1为根的情况。

2 的时候有2棵树，1为根的情况 dp\[0] \* dp\[1] + 2为根的情况 dp\[1] \* dp\[0]

3 的时候有5棵树，1为根的情况 dp\[0] \* dp\[2] + 2为根的情况 dp\[1] \* dp\[1] + 3为根的情况 dp\[2] \* dp\[0]

如此类推，这也就是dp的递推式了。

```java
public int numTrees(int n) {
    if (n < 2) {
        return 1;
    }

    int[] cnt = new int[n + 1];
    cnt[0] = 1;
    cnt[1] = 1;

    for (int i = 2; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            cnt[i] += cnt[j] * cnt[i - j - 1];
        }
    }

    return cnt[n];
}
```
