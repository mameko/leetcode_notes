# 173 Binary Search Tree Iterator

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling`next()`will return the next smallest number in the BST.

**Note:**`next()`and`hasNext()`should run in average O(1) time and uses O(h) memory, wherehis the height of the tree.

```java
public class BSTIterator {
    Stack<TreeNode> stack;
    TreeNode cur;

    public BSTIterator(TreeNode root) {
        stack = new Stack<>();
        cur = root;
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
    if (cur != null || !stack.isEmpty()) {
        return true;
    }

    return false;
    }

    /** @return the next smallest number */
    public int next() {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }

        TreeNode res = stack.pop();
        cur = res.right;

        return res.val;
    }
}
```
