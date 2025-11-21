# L155 Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

这题跟前面一体[Max deapth](104-maximum-depth-of-binary-tree.md)很像，不过要注意只有一个node的情况。

```java
public int minDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }

    int left = minDepth(root.left);
    int right = minDepth(root.right);

    if (left > 0 && right > 0) {
        return Math.min(left, right) + 1;
    } else if (left > 0) {
        return left + 1;
    } else if (right > 0) {
        return right + 1;
    } else {
        return 1;
    }
}
```
