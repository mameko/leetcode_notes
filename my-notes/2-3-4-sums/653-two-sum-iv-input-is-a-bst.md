# 653 Two Sum IV - Input is a BST

Given a Binary Search Tree and a target number, return true if there exist two elements in the BST such that their sum is equal to the given target.

**Example 1:**

```
Input:

    5
   / \
  3   6
 / \   \
2   4   7

Target = 9
Output: True
```

**Example 2:**

```
Input:

    5
   / \
  3   6
 / \   \
2   4   7

Target = 28
Output: False
```

这题一看想了半天好像还是不能利用BST的性质，其实就是2 Sum，只是多了一步树的遍历而已。btw，怎么遍历都行，这里用了中序。其实递归的，BFS的都行。

```java
public boolean findTarget(TreeNode root, int k) {
    if (root == null) {
        return false;
    }

    Stack<TreeNode> stack = new Stack<>();
    Set<Integer> hs = new HashSet<>();
    TreeNode cur = root;
    while (cur != null || !stack.isEmpty()) {
        while (cur != null) {
            stack.push(cur);
            cur = cur.left;
        }

        cur = stack.pop();      

        if (hs.contains(cur.val)) {
            return true;
        } else {
            hs.add(k - cur.val);
        }           

        cur = cur.right;
    }

    return false;
}
```

再来一个递归的：

```java
public boolean findTarget(TreeNode root, int k) {
    Set < Integer > set = new HashSet();
    return find(root, k, set);
}
public boolean find(TreeNode root, int k, Set < Integer > set) {
    if (root == null)
        return false;
    if (set.contains(k - root.val))
        return true;
    set.add(root.val);
    return find(root.left, k, set) || find(root.right, k, set);
}
```
