# 99 Recover Binary Search Tree

Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

**Note:**

A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?

这题考中序遍历，这里我用了stack，所以空间复杂度是O(n)。题目里的follow up要用morris tree。

```java
public void recoverTree(TreeNode root) {
    if (root == null) {
        return;
    }

    Stack<TreeNode> stack = new Stack<>();
    TreeNode cur = root;
    TreeNode previous = null;
    TreeNode p1 = null;
    TreeNode p2 = null;

    while (cur != null || !stack.isEmpty()) {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }

        cur = stack.pop();
        if (previous != null) {
            if (previous.val > cur.val) {
                if (p1 == null) {
                    p1 = previous;
                }
                p2 = cur;
            }
        }
        previous = cur;
        cur = cur.right;
    }

    int tmp = p1.val;
    p1.val = p2.val;
    p2.val = tmp;
}
```
