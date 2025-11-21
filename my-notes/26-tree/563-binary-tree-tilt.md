# 563 Binary Tree Tilt

Given a binary tree, return the tilt of the **whole tree**.

The tilt of a **tree node** is defined as the **absolute difference** between the sum of all left subtree node values and the sum of all right subtree node values. Null node has tilt 0.

The tilt of the **whole tree** is defined as the sum of all nodes' tilt.

**Example:**

```
Input: 
         1
       /   \
      2     3

Output: 1

Explanation:

Tilt of node 2 : 0
Tilt of node 3 : 0
Tilt of node 1 : |2-3| = 1
Tilt of binary tree : 0 + 0 + 1 = 1
```

**Note:**

1. The sum of node values in any subtree won't exceed the range of 32-bit integer.
2. All the tilt values won't exceed the range of 32-bit integer.

这题最难的地方是理解题意，一开始以为就是把左右节点相减就能得到tilt。然后看了其他人的例子，才知道要把整棵左子树的和减去整棵右子树的和。让后用了一个resultType解决了。T：O(n)遍历整棵树， S：O(n)递归栈的深度，树的高度。

```java
class Solution {
    public int findTilt(TreeNode root) {
        if (root == null) {
            return 0;
        }

        ResultType res = findTiltHelper(root);
        return res.tilt;
    }

    private ResultType findTiltHelper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, 0);
        }

        ResultType leftRes = findTiltHelper(root.left);
        ResultType rightRes = findTiltHelper(root.right);

        int curTilt = Math.abs(leftRes.sum - rightRes.sum);
        int curSum = leftRes.sum + rightRes.sum + root.val;

        return new ResultType(curSum, curTilt + leftRes.tilt + rightRes.tilt);
    }
}

class ResultType {
    int sum;
    int tilt;

    public ResultType(int s, int t) {
        sum = s;
        tilt = t;
    }
}
```
