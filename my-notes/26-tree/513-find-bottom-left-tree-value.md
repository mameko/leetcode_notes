# 513 Find Bottom Left Tree Value

Given a binary tree, find the leftmost value in the last row of the tree.

**Example 1:**

```
Input:

    2
   / \
  1   3

Output:
1
```

**Example 2:**

```
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```

**Note:**&#x59;ou may assume the tree (i.e., the given root node) is not **NULL**.

用bfs做层遍历，然后把每一层开始的node的value记下来就ok了。

```java
public int findBottomLeftValue(TreeNode root) {
    if (root == null) {
        return Integer.MIN_VALUE;
    }

    int leftMost = root.val;

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();
            if (i == 0) {
                leftMost = cur.val;
            }

            if (cur.left != null) {
                queue.offer(cur.left);
            }

            if (cur.right != null) {
                queue.offer(cur.right);
            }
        }
    }

    return leftMost;

}
```
