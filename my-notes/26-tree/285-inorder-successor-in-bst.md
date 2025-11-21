# 285 Inorder Successor in BST

Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/23/285_example_1.PNG)

```
Input: root = [2,1,3], p = 1
Output: 2
Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/23/285_example_2.PNG)

```
Input:root = [5,3,6,2,4,null,null,1], p = 6
Output: null
Explanation:There is no in-order successor of the current node, so the answer is null.
```

**Note:**

1. If the given node has no in-order successor in the tree, return`null`.
2. It's guaranteed that the values of the tree are unique.

```java
// 迭代
public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    TreeNode suc = null;
    while (root != null && p != root) {// find location of p and record posible suc from top
        if (root.val > p.val) {
            suc = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }

    if (root == null) {// p not in tree
        return null;
    }

    if (root.right == null) {// p's right sub tree not exist, so suc must be from top
        return suc;
    }

    root = root.right;
    while (root.left != null) {// p's right subtree exsit, suc is the left most node in right subtree
        root = root.left;
    }

    return root;
}

// 递归
public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    if (root == null || p == null) {
        return null;
    }

    if (p.val >= root.val) { // 当要找的大于等于跟，我们去右边找
        return inorderSuccessor(root.right, p); 
    } else {// 如果不是的话，那么在左边找
       TreeNode found = inorderSuccessor(root.left, p);
       return found == null ? root : found;// 如果在左边找到就返回，找不到证明根就是successor
    }
}
```
