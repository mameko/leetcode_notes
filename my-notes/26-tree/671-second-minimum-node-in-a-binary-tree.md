# 671 Second Minimum Node In a Binary Tree

Given a non-empty special binary tree consisting of nodes with the non-negative value, where each node in this tree has exactly`two`or`zero`sub-node. If the node has two sub-nodes, then this node's value is the smaller value among its two sub-nodes.

Given such a binary tree, you need to output the **second minimum** value in the set made of all the nodes' value in the whole tree.

If no such second minimum value exists, output -1 instead.

**Example 1:**

```
Input: 
    2
   / \
  2   5
     / \
    5   7

Output: 5

Explanation: The smallest value is 2, the second smallest value is 5.
```

**Example 2:**

```
Input: 
    2
   / \
  2   2

Output: -1

Explanation: The smallest value is 2, but there isn't any second smallest value.
```

看到这题感觉想起了[414 Third Maximum Number](../quick-sort/414-third-maximum-number.md)，就是套了个树的马甲。就最普通的遍历一遍，然后记录min1，min2的时候记得去重。

```java
public int findSecondMinimumValue(TreeNode root) {
    if (root == null) {
        return 0;
    }

    int min1 = Integer.MAX_VALUE;
    int min2 = Integer.MAX_VALUE;

    Stack<TreeNode> stack = new Stack<>();                       
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode cur = stack.pop();
        if (cur.val != min1 && cur.val != min2) {
            if (cur.val < min1) {
                min2 = min1;
                min1 = cur.val;
            } else if (cur.val < min2) {
                min2 = cur.val;
            }
        }

        if (cur.right != null) {
            stack.push(cur.right);
        }

        if (cur.left != null) {
            stack.push(cur.left);
        }
    }

    return min2 == Integer.MAX_VALUE ? -1 : min2;
}
```
