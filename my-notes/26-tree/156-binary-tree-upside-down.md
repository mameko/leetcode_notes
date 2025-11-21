# 156 Binary Tree Upside Down

Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

For example:

Given a binary tree`{1,2,3,4,5}`,

```
    1
   / \
  2   3
 / \
4   5
```

return the root of the binary tree`[4,5,2,#,#,3,1]`.

```
   4
  / \
 5   2
    / \
   3   1
```

感觉这题比较像改高级链表，要用4个指针，也有递归的方法。

```java
public TreeNode upsideDownBinaryTree(TreeNode root) {
    if (root == null) {
        return null;
    }

    TreeNode pre = null;
    TreeNode right = null;
    TreeNode cur = root;

    while (cur != null) {
        TreeNode next = cur.left;
        cur.left = right;
        right = cur.right;
        cur.right = pre;

        pre = cur;
        cur = next;
    }

    return pre;
}
```

另外递归做法比较像reverse linkedlist

```java
public TreeNode upsideDownBinaryTree(TreeNode root) {
    if (root == null) {
        return null;
    }

    return helper(root);
}

private TreeNode helper(TreeNode root) {
    if (root.left == null) { // 判断左节点而不是根
        return root;
    }

    TreeNode newRoot = helper(root.left); // 这样就可以返回最左节点了
    root.left.left = root.right;
    root.left.right = root;
    root.left = null; // 记得断开
    root.right = null;

    return newRoot;
}
```
