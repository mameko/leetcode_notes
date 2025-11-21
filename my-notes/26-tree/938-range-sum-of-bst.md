# 938 Range Sum of BST

Given the `root` node of a binary search tree, return _the sum of values of all nodes with a value in the range `[low, high]`_.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/11/05/bst1.jpg)

```
Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
Output: 32
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/11/05/bst2.jpg)

```
Input: root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
Output: 23
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 2 * 104]`.
* `1 <= Node.val <= 105`
* `1 <= low <= high <= 105`
* All `Node.val` are **unique**.

嘛，简单题，就是套中序遍历模板。但我竟然背不出来。S：O(h)，T：O（n）。其实这一题直接用递归很快就完事

```java
public int rangeSumBST(TreeNode root, int low, int high) {
    if (root == null) {
        return 0;
    }
 
    Stack<TreeNode> stack = new Stack<>();
    TreeNode cur = root;
    int sum = 0;
    
    while (!stack.isEmpty() || cur != null) {            
        while(cur != null) {
            stack.push(cur);
            cur = cur.left;
        }           
        
        cur = stack.pop();
        int val = cur.val;
        if (val >= low && val <= high) {
            sum += val;
        }
        
        cur = cur.right;
    }
    return sum;        
}   
```
