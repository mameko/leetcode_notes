# 235 Lowest Common Ancestor of a BST

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the[definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow**a node to be a descendant of itself**).”

```
        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
```

For example, the lowest common ancestor (LCA) of nodes`2`and`8`is`6`. Another example is LCA of nodes`2`and`4`is`2`, since a node can be a descendant of itself according to the LCA definition.

这题，因为是个BST，所以可以通过两个点的值的大小来确定LCA。如果是两个root的值在两个点中间，证明那个是LCA，因为在从上往下找。如果两个值都大于根的值，证明两个点都在右子树，同理，如果小于，两个点都在左子树。递归向下求解即可。

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) {
        return root;
    }

    if (p.val <= root.val && q.val >= root.val || p.val >= root.val && q.val <= root.val) {
        return root;
    } else if (p.val > root.val && q.val > root.val) {
        return lowestCommonAncestor(root.right, p, q);
    } else if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestor(root.left, p, q);
    }

    return null;
}
```

非递归版：

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null) {
        return root;
    }

    while (root != null) {
        if (root.val > Math.max(p.val, q.val)) {
            root = root.left;
        } else if (root.val < Math.min(p.val, q.val)) {
            root = root.right;
        } else {
            break;
        }
    }

    return root;
}
```
