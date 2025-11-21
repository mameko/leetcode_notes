# 1022 Sum of Root To Leaf Binary Numbers



You are given the `root` of a binary tree where each node has a value `0` or `1`. Each root-to-leaf path represents a binary number starting with the most significant bit.

* For example, if the path is `0 -> 1 -> 1 -> 0 -> 1`, then this could represent `01101` in binary, which is `13`.

For all leaves in the tree, consider the numbers represented by the path from the root to that leaf. Return _the sum of these numbers_.

The test cases are generated so that the answer fits in a **32-bits** integer.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/04/04/sum-of-root-to-leaf-binary-numbers.png)

```
Input: root = [1,0,1,0,1,0,1]
Output: 22
Explanation: (100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
```

**Example 2:**

```
Input: root = [0]
Output: 0
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 1000]`.
* `Node.val` is `0` or `1`.

这题，一看path sum类型的，就想到递归了。看到它return int有想过从leaf到root，但发现好像不太好做。所以就从root到leaf了。因为数字都是从最左边开始算的，譬如，110 = 1 \* 2 + 1 \* 2 + 0 = 6。每当我们到达leaf的时候，算一下总和。每下去一层，就算一下现在这个要加的数字是多少。T:O(N)，因为要经过每一个node，S:O(H)，递归深度。

解法一：遍历然后加起来

```java
    int total = 0;
    public int sumRootToLeaf(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        recursiveNumberAccumulator(0, root);
        return total;
    }
    
    private void recursiveNumberAccumulator(int fromLastLayer, TreeNode root) {
        if (root == null) {
            return;
        }
        
        int curValue = fromLastLayer * 2 + root.val;
        
        if (root.left == null && root.right == null) {
            total = total + curValue;
        } else {
            recursiveNumberAccumulator(curValue, root.left);
            recursiveNumberAccumulator(curValue, root.right);
        }
        
    }
```

解法二：Morris tree，在solution里看到了这个

