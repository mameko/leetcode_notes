# 106 Construct Binary Tree form Inorder and Postorder Traversal

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**\
You may assume that duplicates do not exist in the tree.

这题跟[105](105-construct-binary-tree-from-preorder-and-inorder-traversal.md)很像，一看，这不就是倒着来嘛。我还是一如既往地懒，用global index来解决了。这里因为是postorder，所以要注意index从n-1开始一直减少到0.然后构建树时记得倒着来。这里同样每次用O(n)找断开位置，同样可以用hashmap优化。

```java
int index;
public TreeNode buildTree(int[] inorder, int[] postorder) {
    if (inorder == null || postorder == null || inorder.length == 0 || postorder.length == 0 
        || inorder.length != postorder.length) {
        return null;
    }

    index = postorder.length - 1;
    return build(postorder, inorder, 0, inorder.length - 1);
}

private TreeNode build(int[] post, int[] in, int start, int end) {
    if (index < 0 || start > end) {
        return null;
    }
    int rootLoc = findRootLoc(in, post[index]);
    TreeNode root = new TreeNode(post[index]);
    index--;
    root.right = build(post, in, rootLoc + 1, end);
    root.left = build(post, in, start, rootLoc - 1);

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
