# 250 Count Univalue Subtrees

Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.

For example:\
Given binary tree,

```
              5
             / \
            1   5
           / \   \
          5   5   5
```

return`4`.

跟前面的subtree题很相似，都是从下往上判定，这里的条件写得有些乱。

```java
int cnt = 0;
public int countUnivalSubtrees(TreeNode root) {
    if (root == null) {
        return 0;
    }

    helper(root);

    return cnt;
}

private boolean helper(TreeNode root) {
    if (root == null) {
        return true;
    }

    boolean left = helper(root.left);
    boolean right = helper(root.right);

    if (!left || !right) {
        return false;
    }

    if (root.left == null && root.right == null) { 
        cnt++;
        return true;
    } else if (root.left != null && root.right == null) {
        if (root.left.val == root.val) {
            cnt++;
            return true;
        }
    } else if (root.left == null && root.right != null) {
        if (root.right.val == root.val) {
            cnt++;
            return true;
        }
    } else {
        if (root.left.val == root.right.val && root.left.val == root.val) {
            cnt++;
            return true;
        }
    }
    return false;
}
```

在discuss上找到一个写得比较好的：

```java
public int countUnivalSubtrees(TreeNode root) {
    int[] count = new int[1];
    helper(root, count);
    return count[0];
}

private boolean helper(TreeNode node, int[] count) {
    if (node == null) {
        return true;
    }
    boolean left = helper(node.left, count);
    boolean right = helper(node.right, count);
    if (left && right) {
        if (node.left != null && node.val != node.left.val) {
            return false;
        }
        if (node.right != null && node.val != node.right.val) {
            return false;
        }
        count[0]++;
        return true;
    }
    return false;
}
```
