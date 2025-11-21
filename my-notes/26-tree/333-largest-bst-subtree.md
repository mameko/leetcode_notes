# 333 Largest BST Subtree

Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

**Note:**\
A subtree must include all of its descendants.\
Here's an example:

```
    10
    / \
   5  15
  / \   \ 
 1   8   7
```

The Largest BST Subtree in this case is the highlighted one.

The return value is the subtree's size, which is 3.

**Hint:**

1. You can recursively use algorithm similar to [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) at each node of the tree, which will result in O(nlogn) time complexity.

**Follow up:**\
Can you figure out ways to solve it with O(n) time complexity?

这题的确跟[98](98-valid-binary-search-tree.md)很像，但是这题要从leaf往上做才能达到O（n）

```java
    int max = 0;
    public int largestBSTSubtree(TreeNode root) {
        if (root == null) {
            return max;
        }

        helper(root);

        return max;
    }

    private ResultType helper(TreeNode root) {
        if (root == null) {
            // return size = 0, -infinity, +infinity - so that we can return the leaf value up
            return new ResultType(0, Integer.MAX_VALUE, Integer.MIN_VALUE);
        }

        ResultType left = helper(root.left);
        ResultType right = helper(root.right);

        // -1 means invalid BST, so we stop adding the nodes up if either subtree is not BST
        // if root's value is smaller than high of left || bigger than low of right
        // means it's not a BST, return -1 up
        if (left.size == -1 || right.size == -1 || root.val <= left.hight || root.val >= right.low) {
            return new ResultType(-1, 0, 0);// doesn't matter what the bound is for non BST
        }

        // if we are here means, both left & right & current node is valid BST, add them up
        int curSize = left.size + right.size + 1;
        max = Math.max(max, curSize); // update global max

        // return the size, and min
        return new ResultType(curSize, Math.min(left.low, root.val), Math.max(right.hight, root.val));
    }
}

class ResultType {
    int size; // size of this subtree
    int low; // the lower bound that's valid
    int hight; // the upper bound that's valid

    public ResultType(int s, int l, int h) {
        size = s;
        low = l;
        hight = h;
    }
}
```
