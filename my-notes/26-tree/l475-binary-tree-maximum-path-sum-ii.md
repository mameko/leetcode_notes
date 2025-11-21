# L475 Binary Tree Maximum Path Sum II

Given a binary tree, find the maximum path sum from root.

The path may end at any node in the tree and contain at least one node in it.

**Example**

Given the below binary tree:

```
  1
 / \
2   3
```

return`4`. (1->3)

递归到叶子，然后把大的那个往上传，边传边加上root的值。

```java
public int maxPathSum2(TreeNode root) {
    if (root == null) {
        return Integer.MIN_VALUE;
    }

    int left = maxPathSum2(root.left);
    int right = maxPathSum2(root.right);

    return Math.max(Math.max(left, right), 0) + root.val;
}
```
