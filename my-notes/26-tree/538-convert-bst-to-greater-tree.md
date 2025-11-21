# 538 Convert BST to Greater Tree

Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

**Example:**

```
Input: The root of a Binary Search Tree like this:
              5
            /   \
           2     13


Output: The root of a Greater Tree like this:
             18
            /   \
          20     13
```

这里是一个reverse inorder traversal + suffix sum。

```java
public TreeNode convertBST(TreeNode root) {
    if (root == null) {
        return root;
    }

    revInorder(root, 0);

    return root;
}

private int revInorder(TreeNode root, int suffixSum) {
    if (root == null) {
        return suffixSum; //老是忘了，return的不是0
    }

    int rightVal = revInorder(root.right, suffixSum);
    root.val += rightVal;
    int leftVal = revInorder(root.left, root.val);

    return leftVal;
}
```
