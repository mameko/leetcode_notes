# 617 Merge Two Binary Trees

You are given two binary trees `root1` and `root2`.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return _the merged tree_.

**Note:** The merging process must start from the root nodes of both trees.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

<pre><code><strong>Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
</strong><strong>Output: [3,4,5,5,4,null,7]
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: root1 = [1], root2 = [1,2]
</strong><strong>Output: [2,2]
</strong></code></pre>

&#x20;

**Constraints:**

* The number of nodes in both trees is in the range `[0, 2000]`.
* `-104 <= Node.val <= 104`

这题直接递归，写了第二次才发现以前原来也做过。就是两个如果都null返回null，哪边exist返回哪边。最后两边都有，就加起来，加到root1那里。还要递归下去子节点s，然后赋值给root1 too。

```java
// 2023
public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
    if (root1 == null && root2 == null) {
        return null;
    } else if (root1 == null) {
        return root2;
    } else if (root2 == null) {
        return root1;
    }

    TreeNode left = mergeTrees(root1.left, root2.left);
    TreeNode right = mergeTrees(root1.right, root2.right);

    root1.val = root1.val + root2.val;
    root1.left = left;
    root1.right = right;
    return root1;
}

// 2019
public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
    TreeNode root = null;
    if (t1 != null && t2 != null) {
        root = new TreeNode(t1.val + t2.val);
        root.left = mergeTrees(t1.left, t2.left);
        root.right = mergeTrees(t1.right, t2.right);
    } else if (t1 != null) {
        root = t1;            
    } else if (t2 != null) {
        root = t2;
    }
    
    return root;
}
```
