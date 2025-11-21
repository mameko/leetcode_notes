# 199 Binary Tree Right Side View

Given a binary tree, imagine yourself standing on therightside of it, return the values of the nodes you can see ordered from top to bottom.

For example:\
Given the following binary tree,

```
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

You should return`[1, 3, 4]`.

这题用BFS来遍历，每次找到这层的最后一个node就放进结果里。

```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();
            if (i == size - 1) {
                res.add(cur.val);
            }

            if (cur.left != null) {
                queue.offer(cur.left);
            }

            if (cur.right != null) {
                queue.offer(cur.right);
            }
        }
    }

    return res;
}
```
