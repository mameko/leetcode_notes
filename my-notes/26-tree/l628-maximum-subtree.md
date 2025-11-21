# L628 Maximum Subtree

Description

Given a binary tree, find the subtree with maximum sum. Return the root of the subtree.

LintCode will print the node you return as the optimal subtree.\
It's guaranteed that there is only one subtree with maximum sum and the given binary tree is not an empty tree.

Example

Example 1:

```
Input:{1,-5,2,0,3,-4,-5}
Output:3
Explanation:
The tree is look like this:     
        1   
      /   \ 
    -5     2 
    / \   /  \
   0   3 -4  -5
The sum of subtree 3 (only one node) is the maximum. So we return 3.
```

Example 2:

```
Input:{1}
Output:1
Explanation:
The tree is look like this:   
        1
There is one and only one subtree in the tree. So we return 1.
```

很经典的题，先丢左边，再丢右边。每次从下面向上。要注意我们记录那些值。我们要记录每一个subtree的sum，用sum；max subtree的node，用了maxNode；和max subtree的value，用了maxSubValue；每次我们要判断maximal subtree是左子树，右子树，或者是根节点所在的子树。

返回的时候，要注意，sum和maxSubvalue。T: O(n),每个节点都会被访问到，S: O(n)，递归深度

```java
public TreeNode findSubtree(TreeNode root) {
    if (root == null) {
        return root;
    }

    ResultType res = findTreeHelper(root);
    return res.maxNode;
}

private ResultType findTreeHelper(TreeNode root) {
    if (root == null) {
        return new ResultType(null, 0, Integer.MIN_VALUE);
    }

    ResultType leftResult = findTreeHelper(root.left);
    ResultType rightResult = findTreeHelper(root.right);

    int curSubSum = root.val + leftResult.sum + rightResult.sum;
    if (curSubSum < Math.max(leftResult.maxSubValue, rightResult.maxSubValue)) {
         if (leftResult.maxSubValue > rightResult.maxSubValue) {
            return new ResultType(leftResult.maxNode, curSubSum, leftResult.maxSubValue);
        } else {
            return new ResultType(rightResult.maxNode, curSubSum, rightResult.maxSubValue);
        }
    }
   
    return new ResultType(root, curSubSum, curSubSum);
}

class ResultType {
    TreeNode maxNode;
    int sum;
    int maxSubValue;
    public ResultType(TreeNode node, int val, int max) {
        maxNode = node;
        sum = val;
        maxSubValue = max;
    }
}
```

