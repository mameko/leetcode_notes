# 112 Path Sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:

Given the below binary tree and`sum = 22`,

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

return true, as there exist a root-to-leaf path`5->4->11->2`which sum is 22.

自己写的很长...=\_=...所以把人家写的也贴上来了。

```java
public boolean hasPathSum(TreeNode root, int sum) {
    if (root == null) {
        return false;
    }

    if (root.left == null && root.right == null && sum - root.val == 0) {
        return true;
    }

    return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
}
```

下面的是自己的解法s。

```java
private boolean helper(TreeNode root, int target, int sumSoFar) {
    if (root == null) {
        return false;
    }

    sumSoFar += root.val;

    if (root.left == null && root.right == null) {
        if (sumSoFar == target) {
            return true;
        }
    }

    boolean left = helper(root.left, target, sumSoFar);
    boolean right = helper(root.right, target, sumSoFar);

    return left || right;
}
```

```java
public boolean hasPathSum(TreeNode root, int sum) {
    if (root == null) {
        return false;
    }

    return helper(root, sum, 0);
}

private boolean helper(TreeNode root, int target, int sumSoFar) {
    sumSoFar += root.val;

    if (root.left == null && root.right == null) {
        if (sumSoFar == target) {
            return true;
        }
    }

    boolean left = false;
    if (root.left != null) {
         left = helper(root.left, target, sumSoFar);
    }

    boolean right = false;
    if (root.right != null) {
        right = helper(root.right, target, sumSoFar);
    }

    return left || right;
}
```
