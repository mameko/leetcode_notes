# 814 Binary Tree Pruning

We are given the head node`root` of a binary tree, where additionally every node's value is either a 0 or a 1.

Return the same tree where every subtree (of the given tree) not containing a 1 has been removed.

(Recall that the subtree of a node X is X, plus every node that is a descendant of X.)

```
Example 1:
Input: [1,null,0,0,1]
Output: [1,null,0,null,1]

1                 1
 \                 \ 
  0       ==>       0
 / \                 \
0   1                 1

Explanation: 
Only the red nodes satisfy the property "every subtree not containing a 1".
The diagram on the right represents the answer.
```

```
Example 2:
Input: [1,0,1,0,0,0,1]
Output: [1,null,1,null,1]

      1           1
    /  \           \
   0    1   ==>     1
  / \  / \           \ 
 0  0  0  1           1
```

```
Example 3:
Input: [1,1,0,1,1,0,1,0]
Output: [1,1,0,1,1,null,1]

      1             1 
     / \           /  \
    1   1     ==> 1    1
   / \ / \       / \    \
   1 1 0 1       1  1    1
  /
 1
```

**Note:**

* The binary tree will have at most`100 nodes`.
* The value of each node will only be`0`or`1`.

这题一看还以为要返回2个值，所以还用个resulttype，然后想想好像不用。判断时要注意最后才判断是否1，如果判断太早没改好tree。

```java
public TreeNode pruneTree(TreeNode root) {
    if (root == null) {
        return root;
    }

    helper(root);
    return root;
}

private boolean helper(TreeNode root) {
    if (root == null) {
        return false;
    }


    boolean left = helper(root.left);
    boolean right = helper(root.right);    


    if (left == false) { //没有1，所以砍掉左子树
        root.left = null;            
    } 

    if (right == false) {//没有1，所以砍掉子右子树
        root.right = null;
    }

     if (root.val == 1) {// 如果这个点是1，我们直接返回true
        return true;
    }
    return left || right; // 如果为0，要看左或右子树是否有1，只要有一个有1，就返回true，不砍              
}
```
