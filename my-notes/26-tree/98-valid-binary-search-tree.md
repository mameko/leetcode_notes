# 98 Valid Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.

**Example 1:**

```
    2
   / \
  1   3
```

Binary tree`[2,1,3]`, return true.

**Example 2:**

```
    1
   / \
  2   3
```

Binary tree`[1,2,3]`, return false.

原来还可以用null =\_=|||...

```java
public boolean isValidBST(TreeNode root) {
    if (root == null) {
        return true;
    }

    return helper(root, null, null);
}

private boolean helper(TreeNode root, Integer min, Integer max) {
    if (root == null) {
        return true;
    }

    if ((min != null && min >= root.val) || (max != null && max <= root.val)) {
        return false;
    }

    boolean left = helper(root.left, min, root.val);
    boolean right = helper(root.right, root.val, max);

    return left && right;
}
```

这题因为要考虑到corner case所以要用long。不然\[Integer.MAX]的test case会过不了。

```java
public boolean isValidBST(TreeNode root) {
    if (root == null) {
        return true;
    }

    return helper(root, Long.MIN_VALUE, Long.MAX_VALUE);
}

private boolean helper(TreeNode root, Long min, Long max) {
    if (root == null) {
        return true;
    }

    if (min >= root.val || max <= root.val) {
        return false;
    }

    boolean left = helper(root.left, min, (long) root.val);
    boolean right = helper(root.right, (long) root.val, max);

    return left && right;
}
```

很久前写的O（n），还能iterative地写

```java
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        Result r = new Result();
        isValidBSTHelper(root, r);

        return r.isBST;
    }

    private void isValidBSTHelper(TreeNode root, Result r) {
         if (root == null || r.isBST == false) {
            return;
        }

        isValidBSTHelper(root.left, r);
        if (r.isBST == false) {
            return;
        }
        if (r.last != null && r.last.val >= root.val) {
            r.isBST = false;
            return;
        }
        r.last = root;
        isValidBSTHelper(root.right, r);
    }


class Result {
    TreeNode last;
    Boolean isBST = true;
}
```
