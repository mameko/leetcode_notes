# L578 Lowest Common Ancestor III

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.\
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.\
Return`null`if LCA does not exist.

## Notice

node A or node B may not exist in tree.

**Example**

For the following binary tree:

```
  4
 / \
3   7
   / \
  5   6
```

LCA(3, 5) =`4`

LCA(5, 6) =`7`

LCA(6, 7) =`7`

这题可以用-1来表示没找到，不过老师说用一个ResultType类来表示会好一点。基本做法跟[LCA I](236-lowest-common-ancestor-of-a-binary-tree.md)差不多，还是分治。

```java
    public TreeNode lowestCommonAncestor3(TreeNode root, TreeNode A, TreeNode B) {
        if (root == null || A == null || B == null) {
            return null;
        }

        ResultType res = helper(root, A, B);
        if (res.aExist && res.bExist) {
            return res.node;
        }

        return null;
    }

    private ResultType helper(TreeNode root, TreeNode A, TreeNode B) {
        ResultType res = new ResultType(false, false, null);
        if (root == null) {
            return res;
        }

        ResultType left = helper(root.left, A, B);
        ResultType right = helper(root.right, A, B);

        boolean aExist = left.aExist || right.aExist || root == A;
        boolean bExist = left.bExist || right.bExist || root == B;

        if (A == root || B == root) {
            return new ResultType(aExist, bExist, root);
        }

        if (left.node != null && right.node != null) {
            return new ResultType(aExist, bExist, root);
        } else if (left.node != null) {
            return new ResultType(aExist, bExist, left.node);
        } else if (right.node != null) {
            return new ResultType(aExist, bExist, right.node);
        } else {
            return res;
        }
    }


class ResultType {
    boolean aExist;
    boolean bExist;
    TreeNode node;

    public ResultType(boolean a, boolean b, TreeNode n) {
        aExist = a;
        bExist = b;
        node = n;
    }
}
```
