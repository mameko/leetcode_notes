# 107 Binary Tree Level Order Traversal II

Given a binary tree, return thebottom-up level ordertraversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:\
Given binary tree`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
```

还是bfs...嘛...用了java自带的翻转功能，所以不太难。

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int size = queue.size();
        ArrayList<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();
            level.add(cur.val);

            if (cur.left != null) {
                queue.offer(cur.left);
            }

            if (cur.right != null) {
                queue.offer(cur.right);
            }
        }
        res.add(level);
    }

    Collections.reverse(res);
    return res;
}
```
