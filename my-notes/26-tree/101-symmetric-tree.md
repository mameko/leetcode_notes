# 101 Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree`[1,2,2,3,4,4,3]`is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following`[1,2,2,null,3,null,3]`is not:

```
    1
   / \
  2   2
   \   \
   3    3
```

**Note:**\
Bonus points if you could solve it both recursively and iteratively.

```java
public boolean isSymmetric(TreeNode root) {
    if (root == null) {
        return true;
    }

    boolean res = isSymHelper(root.left, root.right);
    return res;
}

private boolean isSymHelper(TreeNode a, TreeNode b) {
    if (a == null && b == null) {
        return true;
    } else if (a == null || b == null) {
        return false;
    }

    if (a.val != b.val) {
        return false;
    }

    boolean LR = isSymHelper(a.left, b.right);
    boolean RL = isSymHelper(a.right, b.left);

    if (LR && RL) {
        return true;
    }

    return false;
}
```
