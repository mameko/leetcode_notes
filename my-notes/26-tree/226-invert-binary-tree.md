# 226 Invert Binary Tree

Invert a binary tree.

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

to

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

先递归到底层，把下面的换好，然后再换上面的。

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }

    helper(root);

    return root;
}

private void helper(TreeNode root) {
    if (root == null) {
        return;
    }

    helper(root.left);
    helper(root.right);

    TreeNode tmp = root.left;
    root.left = root.right;
    root.right = tmp;
}
```
