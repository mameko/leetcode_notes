# L596 minimum Subtree

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

## Notice

LintCode will print the subtree which root is your return node.\
It's guaranteed that there is only one subtree with minimum sum and the given binary tree is not an empty tree.

**Example**

Given a binary tree:

```
     1
   /   \
 -5     2
 / \   /  \
0   2 -4  -5
```

return the node`1`.

很经典，就是从leaf把值往上传，然后每到一个节点算一下sum，如果比global的小就更新一下。

```java
    int min = Integer.MAX_VALUE;
    TreeNode minNode = null;
    public TreeNode findSubtree(TreeNode root) {
        if (root == null) {
            return null;
        }

        helper(root);

        return minNode;
    }

    private int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = helper(root.left);
        int right = helper(root.right);

        int curSum = left + right + root.val;
        if (curSum < min) {
            min = curSum;
            minNode = root;
        }

        return curSum;
    }
```
