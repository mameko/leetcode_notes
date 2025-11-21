# 103 Binary Tree Zigzag Level Order Traversal

Given a binary tree, return thezigzag level ordertraversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:\
Given binary tree`[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
  [15,7]
]
```

还是bfs加java自带翻转功能

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    boolean forward = true;

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

        if (forward) {
            forward = false;
        } else {
            forward = true;
            Collections.reverse(level);
        }

        res.add(level);
    }

    return res;
}
```
