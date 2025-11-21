# 366 Find Leaves of Binary Tree

Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

**Example:**\
Given binary tree

```
          1
         / \
        2   3
       / \     
      4   5
```

Returns`[4, 5, 3], [2], [1]`.

**Explanation:**

1. Removing the leaves`[4, 5, 3]`would result in this tree:

```
          1
         / 
        2
```

1. Now removing the leaf`[2]`would result in this tree:

```
          1
```

1. Now removing the leaf`[1]`would result in the empty tree:

```
          []
```

Returns`[4, 5, 3], [2], [1]`.

一层一层地剥，记得要返回，因为java值传递。

```java
public List<List<Integer>> findLeaves(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    // add leave level by level
    while (root != null) {
        ArrayList<Integer> level = new ArrayList<>();
        root = remove(root, level);
        res.add(level);
    }

    return res;
}

private TreeNode remove(TreeNode root, ArrayList<Integer> level) {
    if (root == null) { // 这里判空是因为，如果树有单边的情况，就像例子里的1，2的时候，
        return null;    // 右子树会继续递归，所以不判断一下下面那个if会nullpointerexception
    }

    // if encounter leaves, add to level arraylist, then "remove" it by returning null
    if (root.left == null && root.right == null) {
        level.add(root.val);
        return null;
    }

    // if node encounter is not a leave yet, we recur to find leave
    root.left = remove(root.left, level);
    root.right = remove(root.right, level);

    return root;
}
```
