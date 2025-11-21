# 314 Binary Tree Vertical Order Traversal

Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from **left to right**.

**Examples:**

1.  Given binary tree\
    `[3,9,20,null,null,15,7]`\
    ,

    ```
       3
      /\
     /  \
     9  20
        /\
       /  \
      15   7
    ```

return its vertical order traversal as:

```
   [
     [9],
     [3,15],
     [20],
     [7]
   ]
```

1.  Given binary tree\
    `[3,9,8,4,0,1,7]`\
    ,

    ```
         3
        /\
       /  \
       9   8
      /\  /\
     /  \/  \
     4  01   7
    ```

return its vertical order traversal as:

```
   [
     [4],
     [9],
     [3,0,1],
     [8],
     [7]
   ]
```

1.  Given binary tree\
    `[3,9,8,4,0,1,7,null,null,null,2,5]`\
    (0's right child is 2 and 1's left child is 5),

    ```
         3
        /\
       /  \
       9   8
      /\  /\
     /  \/  \
     4  01   7
        /\
       /  \
       5   2
    ```

return its vertical order traversal as:

```
   [
     [4],
     [9,5],
     [3,0,1],
     [8,2],
     [7]
   ]
```

这题不看答案完全想不出，而且也没看懂想干嘛。原来是把每个节点按层遍历，遍历时记录和根之间的距离是多少。左移一格-1，右移一格+1.

```java
   public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }

        Map<Integer, List<Integer>> map = new TreeMap<>();
        Queue<Pair> queue = new LinkedList<>();
        queue.offer(new Pair(root, 0));

        while (!queue.isEmpty()) {
            Pair cur = queue.poll();

            List<Integer> tmp;
            if (map.containsKey(cur.col)) {
                tmp = map.get(cur.col);
            } else {
                tmp = new ArrayList<>();
            }

            tmp.add(cur.node.val);
            map.put(cur.col, tmp);

            if (cur.node.left != null) {
                queue.offer(new Pair(cur.node.left, cur.col - 1));
            }

            if (cur.node.right != null) {
                queue.offer(new Pair(cur.node.right, cur.col + 1));
            }
        }

        for (Map.Entry<Integer, List<Integer>> entry : map.entrySet()) {
            res.add(entry.getValue());
        }

        return res;
    }
}

class Pair {
    TreeNode node;
    int col;

    public Pair(TreeNode n, int c) {
        node = n;
        col = c;
    }
}
```
