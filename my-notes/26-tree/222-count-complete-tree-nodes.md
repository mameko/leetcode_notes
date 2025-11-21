# 222 Count Complete Tree Nodes

Given a **complete** binary tree, count the number of nodes.

**Definition of a complete binary tree from** [**Wikipedia**](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**:**\
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

这题如果用bfs直接数O(n)会超时，所以只能向logn靠拢。因为这个是满二叉树，所以我们用了logn去数高度，然后再logn去数右子树。T：O(logn ^ 2). 这一题3年想了3次，感觉不太能记住推导过程，只能记住结论了。这里的结论是，每一次循环，都是把除了现在这个点以外的另一边的点全加进去。这个数就是2的h或2的h - 1次方个点。

![](<../../.gitbook/assets/image (11).png>)

```java
public int countNodes(TreeNode root) {
    if (root == null) {
        return 0;
    }

    int cnt = 0;
    // get the height of the tree
    int h = cntHeight(root);

    while (root != null) {
        // right side is full, if right's height is 1 less than root
        if (cntHeight(root.right) == h - 1) {
            cnt += 1 << h;// if right side is full we add 2 ^ h node to result
            root = root.right;
        } else {
        // right side is not full, so we go count the left side
            cnt += 1 << h - 1;// if not full, we add 2 ^ (h - 1) node to result
            root = root.left;
        }
        h--;// decrease height every time we go down one level
    }

    return cnt;
}

// get height by counting left node, because complete tree
private int cntHeight(TreeNode root) {
    return root == null ? -1 : 1 + cntHeight(root.left);
}
```
