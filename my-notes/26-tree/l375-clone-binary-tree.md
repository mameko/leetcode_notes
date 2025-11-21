# L375 Clone Binary Tree

For the given binary tree, return a**deep copy**of it.

**Example**

Given a binary tree:

```
     1
   /  \
  2    3
 / \
4   5
```

return the new binary tree with same structure and same value:

```
     1
   /  \
  2    3
 / \
4   5
```

非常简单的自底向上copy。

```java
public TreeNode cloneTree(TreeNode root) {
    if (root == null) {
        return null;
    }

    TreeNode left = cloneTree(root.left);
    TreeNode right = cloneTree(root.right);

    TreeNode newRoot = new TreeNode(root.val);
    newRoot.left = left;
    newRoot.right = right;

    return newRoot;
}
```
