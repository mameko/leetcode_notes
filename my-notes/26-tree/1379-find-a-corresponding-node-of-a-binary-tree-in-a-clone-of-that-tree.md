# 1379 Find a Corresponding Node of a Binary Tree in a Clone of That Tree



Given two binary trees `original` and `cloned` and given a reference to a node `target` in the original tree.

The `cloned` tree is a **copy of** the `original` tree.

Return _a reference to the same node_ in the `cloned` tree.

**Note** that you are **not allowed** to change any of the two trees or the `target` node and the answer **must be** a reference to a node in the `cloned` tree.

**Follow up:** Solve the problem if repeated values on the tree are allowed.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/02/21/e1.png)

```
Input: tree = [7,4,3,null,null,6,19], target = 3
Output: 3
Explanation: In all examples the original and cloned trees are shown. The target node is a green node from the original tree. The answer is the yellow node from the cloned tree.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/02/21/e2.png)

```
Input: tree = [7], target =  7
Output: 7
```

**Example 3:**![](https://assets.leetcode.com/uploads/2020/02/21/e3.png)

```
Input: tree = [8,null,6,null,5,null,4,null,3,null,2,null,1], target = 4
Output: 4
```

**Example 4:**![](https://assets.leetcode.com/uploads/2020/02/21/e4.png)

```
Input: tree = [1,2,3,4,5,6,7,8,9,10], target = 5
Output: 5
```

**Example 5:**![](https://assets.leetcode.com/uploads/2020/02/21/e5.png)

```
Input: tree = [1,2,null,3], target = 2
Output: 2
```

**Constraints:**

* The number of nodes in the `tree` is in the range `[1, 10^4]`.
* The values of the nodes of the `tree` are unique.
* `target` node is a node from the `original` tree and is not `null`.

其实这题一看就知道是同时遍历两棵树，可以用bfs队列，或者dfs递归，或者iteration来前中后序遍历。因为遍历模板背的不熟悉，所以选择了用中序stack。T:O(n), S:O(height)

```java
public final TreeNode getTargetCopy(final TreeNode original, final TreeNode cloned, final TreeNode target) {
    if (original == null || cloned == null) {
        return null;
    }

    Stack<TreeNode> stack1 = new Stack<>();
    Stack<TreeNode> stack2 = new Stack<>();
    TreeNode cur1 = original;
    TreeNode cur2 = cloned;
    while (cur1 != null || !stack1.isEmpty()) {
        while (cur1 != null) {
            stack1.push(cur1);
            stack2.push(cur2);
            cur1 = cur1.left;
            cur2 = cur2.left;
        }

        cur1 = stack1.pop();
        cur2 = stack2.pop();

        if (cur1.val == target.val) {
            return cur2;
        }

        cur1 = cur1.right;
        cur2 = cur2.right;
    }

    return null;
}
```
