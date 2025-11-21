# 108 Convert Sorted Array to Binary Search Tree

Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

注意递归要从start 到 mid - 1，mid + 1 到 end，因为mid已经拿出来做root了。

```java
public TreeNode sortedArrayToBST(int[] nums) {
    if (nums == null || nums.length == 0) {
        return null;
    }

    TreeNode root = buildTree(nums, 0, nums.length - 1);
    return root;
}

private TreeNode buildTree(int[]nums, int start, int end) {
    if (start > end) {
        return null;
    }

    int mid = start + (end - start) / 2;
    TreeNode root = new TreeNode(nums[mid]);

    root.left = buildTree(nums, start, mid - 1);
    root.right = buildTree(nums, mid + 1, end);

    return root;
}
```
