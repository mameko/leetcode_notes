# 404 Sum of Left Leaves

Find the sum of all left leaves in a given binary tree.

**Example:**

```
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```

有递归和[非递归版本](https://discuss.leetcode.com/topic/60403/java-iterative-and-recursive-solutions)

```java
public int sumOfLeftLeaves(TreeNode root) {
    int sum = 0;
    if (root == null) {
        return sum;
    }

    // if we have left branch
    if (root.left != null) {
        // if we found a left leaf, add it to sum;
        if (root.left.left == null && root.left.right == null) {
            sum += root.left.val;
        } else {// if we are not at the left leaf, we recur down to find the left leaf
            sum += sumOfLeftLeaves(root.left);
        }
    }

    // after we done with left subtree, we add right subtree's left leaf nodes
    sum += sumOfLeftLeaves(root.right);

    return sum;
}
```
