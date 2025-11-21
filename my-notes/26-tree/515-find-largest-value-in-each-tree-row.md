# 515 Find Largest Value in Each Tree Row

You need to find the largest value in each row of a binary tree.

**Example:**

```
Input:


          1
         / \
        3   2
       / \   \  
      5   3   9 


Output: [1, 3, 9]
```

还是bfs层遍历，然后在每层找一次max，最后返回。

```java
public List<Integer> largestValues(TreeNode root) {
    List<Integer> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        int size = queue.size();
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < size; i++) {
            TreeNode cur = queue.poll();
            max = Math.max(cur.val, max);

            if (cur.left != null) {
                queue.offer(cur.left);
            }

            if (cur.right != null) {
                queue.offer(cur.right);
            }
        }
        res.add(max);
    }

    return res;
}
```
