# L453 Flatten Binary Tree to Linked List

Flatten a binary tree to a fake "linked list" in pre-order traversal.

Here we use the\_right\_pointer in TreeNode as the\_next\_pointer in ListNode.

## Notice

Don't forget to mark the left child of each node to null. Or you will get Time Limit Exceeded or Memory Limit Exceeded.

**Example**

```
              1
               \
     1          2
    / \          \
   2   5    =>    3
  / \   \          \
 3   4   6          4
                     \
                      5
                       \
                        6
```

[**Challenge**](http://www.lintcode.com/en/problem/flatten-binary-tree-to-linked-list/#challenge)

Do it in-place without any extra memory.

这题变相考了非递归的pre order traversal。

```java
public void flatten(TreeNode root) {
    if (root == null) {
        return;
    }

    Stack<TreeNode> s = new Stack<>();
    TreeNode last = null;
    s.push(root);
    while (!s.isEmpty()) {
        TreeNode cur = s.pop();
        if (last != null) {
            last.right = cur;
            last.left = null;
        }
        last = cur;

        if (cur.right != null) {
            s.push(cur.right);
        }

        if (cur.left != null) {
            s.push(cur.left);
        }
    }
}
```
