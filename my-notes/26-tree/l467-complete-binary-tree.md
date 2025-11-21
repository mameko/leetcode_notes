# L467 Complete Binary Tree

Given a Binary Tree, write a function to check whether the given Binary Tree is Complete Binary Tree or not. A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible. See following examples.

用bfs一层一层看，用变量记录是否能有child。

```java
public boolean isComplete(TreeNode root) {
    if (root == null) {
        return true;
    }

    boolean mustNotHasChild = false;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        TreeNode cur = queue.poll();
        // if you must not have child is true, and the node has child, return false;
        if (mustNotHasChild) {
            if (cur.left != null || cur.right != null) {
                return false;
            }
        }
        // if left child & right child are both here, you can put them in queue for future processing
        if (cur.left != null && cur.right != null) {
            queue.offer(cur.left);
            queue.offer(cur.right);
        } else if (cur.left != null && cur.right == null) {
        // if right child is null, means next level you can't have child
            queue.offer(cur.left);
            mustNotHasChild = true;
        } else if (cur.left == null && cur.right != null) {
        // if left is null & right is not null, this is not a complete binary tree, so return false;
            return false;
        } else {
        // last level, so 
            mustNotHasChild = true;
        }
    }

    return true;
}
```
