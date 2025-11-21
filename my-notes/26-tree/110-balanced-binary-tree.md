# 110 Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees ofeverynode never differ by more than 1.

可以另外开一个ResultType，也可以用-1表示不balanced的tree返回值。

```java
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }

        ResultType res = isBalancedHelper(root);

        return res.isBalance;
    }

    private ResultType isBalancedHelper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, true);
        }

        ResultType left = isBalancedHelper(root.left);
        ResultType right = isBalancedHelper(root.right);

        if (!left.isBalance || !right.isBalance) {
            return new ResultType(-1, false);
        }

        if (Math.abs(left.height - right.height) > 1) {
            return new ResultType(-1, false);
        }

        int height = Math.max(left.height, right.height) + 1;
        return new ResultType(height, true);
    }


class ResultType {
    int height;
    boolean isBalance;

    public ResultType(int h, boolean isBalance) {
        height = h;
        this.isBalance = isBalance;
    }
}
```

```java
public boolean isBalanced(TreeNode root) {
    if (root == null) {
        return true;
    }

    int res = helper(root);
    return res != -1;
}

private int helper(TreeNode root) {
    if (root == null) {
        return 0;
    }

    int left = helper(root.left);
    int right = helper(root.right);

    if (left < 0 || right < 0) {
        return -1;
    }

    if (Math.abs(left - right) <= 1) {
        return Math.max(left, right) + 1;
    } else {
        return -1;
    }
}
```
