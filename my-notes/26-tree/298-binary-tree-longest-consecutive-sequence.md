# 298 Binary Tree Longest Consecutive Sequence

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

For example,

```
   1
    \
     3
    / \
   2   4
        \
         5
```

Longest consecutive sequence path is`3-4-5`, so return`3`.

```
   2
    \
     3
    / 
   2    
  / 
 1
```

Longest consecutive sequence path is`2-3`,not`3-2-1`, so return`2`.

用全局变量记录总数，然后把每次下一个数该是什么值传下去递归。如果下一层发现跟传下里的值不一样证明不连续了，所以得重新计数。

```java
int res = 0; 
public int longestConsecutive(TreeNode root) {
    if (root == null) {
        return res;
    }

    helper(0, root, root.val);

    return res;
}

private void helper(int conSofar, TreeNode root, int target) {
    if (root == null) {
        return;
    }

    if (root.val != target) {
        conSofar = 1;
    } else {
        conSofar += 1;
    }
    int nextTarget = root.val + 1;
    res = Math.max(conSofar, res);

    helper(conSofar, root.left, nextTarget);
    helper(conSofar, root.right, nextTarget);

}
```
