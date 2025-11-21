# L597 Subtree with Maximum Average

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

## Notice

LintCode will print the subtree which root is your return node.\
It's guaranteed that there is only one subtree with maximum average.

**Example**

Given a binary tree:

```
     1
   /   \
 -5     11
 / \   /  \
1   2 4    -2
```

return the node`11`.

把子树的节点数目（num）跟子树节点数字的和（val）用一个ResultType来记录，然后往上传。然后在每个点算一算average，更新global max。九章的解法用了乘法来避免出现double。

```java
    double max = Double.MIN_VALUE;
    TreeNode maxNode = null;
    public TreeNode findSubtree2(TreeNode root) {
        if (root == null) {
            return null;
        }

        helper(root);

        return maxNode;
    }

    private ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, 0);
        }

        ResultType left = helper(root.left);
        ResultType right = helper(root.right);

        int curVal = left.val + right.val + root.val;
        int curNum = left.num + right.num + 1;

        double curDiv = (double)curVal / (double)curNum;
            if (curDiv > max) {
                max = curDiv;
                maxNode = root;
            }

        return new ResultType(curNum, curVal);
    }


class ResultType {
    int num;
    int val;

    public ResultType(int n, int v) {
        num = n;
        val = v;
    }
}
```
