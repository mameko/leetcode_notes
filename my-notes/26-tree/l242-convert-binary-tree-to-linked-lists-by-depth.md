# L242 Convert Binary Tree to Linked Lists by Depth

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you'll have D linked lists).

**Example**

Given binary tree:

```
    1
   / \
  2   3
 /
4
```

return

```
[
  1->null,
  2->3->null,
  4->null
]
```

这题可以用bfs和dfs做，bfs是自己写的，dfs是九章的实现。

```java
public List<ListNode> binaryTreeToLists(TreeNode root) {
    List<ListNode> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    // bfs
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int size = queue.size();
        ListNode dummy = new ListNode(-1);
        ListNode p = dummy;
        for (int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();
            ListNode newNode = new ListNode(cur.val);
            p.next = newNode;
            p = p.next;
            if (cur.left != null) {
                queue.offer(cur.left);
            }

            if (cur.right != null) {
                queue.offer(cur.right);
            }
        }
        res.add(dummy.next);
    }

    return res;
}
```

dfs：

```java
public List<ListNode> binaryTreeToLists(TreeNode root) {
    // Write your code here
    List<ListNode> result = new ArrayList<ListNode>();
    dfs(root, 1, result);
    return result;
}

void dfs(TreeNode root, int depth, List<ListNode> result) {
    if (root == null)
        return;
    ListNode node = new ListNode(root.val);
    if (result.size() < depth) {
        result.add(node);
    }
    else {
        node.next = result.get(depth-1);
        result.set(depth-1, node);
    }
    dfs(root.right, depth + 1, result);
    dfs(root.left, depth + 1, result);
}
```
