# L614 Binary Tree Longest Consecutive Sequence II

Given a binary tree, find the length of the longest consecutive sequence path.\
The path could be start and end at any node in the tree

**Example**

```
    1
   / \
  2   0
 /
3
```

Return`4`//`0-1-2-3-4`

做法跟前一题基本一样，这里要判断从左边来的inc/dec与右边来的Inc/Dec哪个大，取大的往上传。要用一个resultType来记录当前节点上升，下降和Max的值。因为up跟down不可能在同一侧同时得到最大值，所以len直接加起来就ok了。up算的是从root到leave，increase，down算的是从root到leave，decrease。其实如果up/down初始化为1的话，最后len要up + down - 1。这题的复杂度是T：O(n),遍历了所有的点； S：O(n)递归深度

```java
    public int longestConsecutive2(TreeNode root) {
        if (root == null) {
            return 0;
        }

        ResultType res = helper(root);
        return res.max;
    }

    private ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, 0, 0);
        }

        ResultType leftRes = helper(root.left);
        ResultType rightRes = helper(root.right);

        int up = 0;
        int down = 0;

        if (root.left != null && root.left.val == root.val + 1) {
            up = Math.max(up, leftRes.up + 1);
        }

        if (root.left != null && root.left.val == root.val - 1) {
            down = Math.max(down, leftRes.down + 1);
        }

        if (root.right != null && root.right.val == root.val + 1) {
            up = Math.max(up, rightRes.up + 1);
        }

        if (root.right != null && root.right.val == root.val - 1) {
            down = Math.max(down, rightRes.down + 1);
        }

        int len = up + down + 1;
        len = Math.max(len, Math.max(leftRes.max, rightRes.max));

        return new ResultType(up, down, len);

    } 


class ResultType {
    int up;
    int down;
    int max;

    public ResultType(int u, int d, int m) {
        up = u;
        down = d;
        max = m;
    }
}
```
