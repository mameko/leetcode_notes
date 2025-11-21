# 100 Same Tree

Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

**Example**

```
    1             1
   / \           / \
  2   2   and   2   2
 /             /
4             4
```

are identical.

```
    1             1
   / \           / \
  2   3   and   2   3
 /               \
4                 4
```

are not identical.

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) {
        return true;
    } else if (p == null || q == null) {
        return false;
    } 

    if (p.val != q.val) {
        return false;
    }

    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```
