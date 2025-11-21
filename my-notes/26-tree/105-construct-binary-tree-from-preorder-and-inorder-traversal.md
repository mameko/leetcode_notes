# 105 Construct Binary Tree from Preorder and Inorder Traversal

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**\
You may assume that duplicates do not exist in the tree.

这题要通过两个数组来建树。在网上看到大神们各种复杂地控制下标。我这里懒了用global变量。解法主要是，因为preorder的第一个一定是root，然后要从inorder那里找下标，把树的左右子树找出来，然后递归建树。每次找下标都得O(n)。另外可以通过控制范围来减少查找时间，不过下标控制各种麻烦，还有就是因为数字没有重复，所以其实可以用O(n)的空间建一个HashMap方便查找。

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    if (preorder == null || inorder == null || preorder.length == 0 || inorder.length == 0 
        || preorder.length != inorder.length) {
        return null;
    }

    return build(preorder, inorder, 0, inorder.length - 1);
}

int index = 0;
private TreeNode build(int[] pre, int[] in, int start, int end) {
    if (index >= pre.length || start > end) {
        return null;
    }
    int rootLoc = findRootLoc(in, pre[index]);
    TreeNode root = new TreeNode(pre[index]);
    index++;
    root.left = build(pre, in, start, rootLoc - 1);
    root.right = build(pre, in, rootLoc + 1, end);

    return root;
}

private int findRootLoc(int[] arr, int val) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == val) {
            return i;
        }
    }
    return -1;
}
```
