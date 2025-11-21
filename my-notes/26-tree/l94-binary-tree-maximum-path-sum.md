# L94 Binary Tree Maximum Path Sum

Given a binary tree, find the maximum path sum.

The path may start and end at any node in the tree.

**Example**

Given the below binary tree:

```
  1
 / \
2   3
```

return`6`.

这题跟前一题基本结构一样，都是从叶子往上返回值。

leetcode discuss的解法是，用以个global的变量来记录全局max，然后每次从子节点返回一个root to 它的max。每层root判断是否把左右根加起来比原来的Max大，如果是就更新。

```java
int max;

public int maxPathSum(TreeNode root) {
    max = Integer.MIN_VALUE;
    maxDownHelper(root);
    return max;
}

private int maxDownHelper(TreeNode root) {
    if (root == null) {
        return 0;
    }

    // because the tree may return minus value, so use 0 to represent not taking the negative node/path
    int left = Math.max(0, maxDownHelper(root.left));
    int right = Math.max(0, maxDownHelper(root.right));

    // leaf node's value will be in the max, becuase null returns 0
    // if left & right returns minus val, the node's value will be used to compare to the global max
    max = Math.max(max, left + right + node.val);

    return Math.max(left, right) + node.val; // return max from leaf to current root
}
```

九章的解法是用一个ResultType来记录两个量，一个是从root to any，另一个是any to any。

```java
    public int maxPathSum(TreeNode root) {
        if (root == null) {
            return 0;
        }

        ResultType res = helper(root);
        return res.maxAnyToAny;
    }

    private ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, Integer.MIN_VALUE);
        }

        ResultType left = helper(root.left);
        ResultType right = helper(root.right);

        int maxRootToAny = Math.max(0, Math.max(left.maxRootToAny + root.val, right.maxRootToAny + root.val));
        int maxAnyToAny = Math.max(left.maxRootToAny + right.maxRootToAny + root.val, Math.max(left.maxAnyToAny, right.maxAnyToAny));


        return new ResultType(maxRootToAny, maxAnyToAny);
    }


class ResultType {
    int maxRootToAny;
    int maxAnyToAny;

    public ResultType(int ra, int aa) {
        maxRootToAny = ra;
        maxAnyToAny = aa;
    }

}
```
