# 987 Vertical Order Traversal of a Binary Tree

Given the `root` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position `(x, y)`, its left and right children will be at positions `(x - 1, y - 1)` and `(x + 1, y - 1)` respectively.

The **vertical order traversal** of a binary tree is a list of non-empty **reports** for each unique x-coordinate from left to right. Each **report** is a list of all nodes at a given x-coordinate. The **report** should be primarily sorted by y-coordinate from highest y-coordinate to lowest. If any two nodes have the same y-coordinate in the **report**, the node with the smaller value should appear earlier.

Return _the **vertical order traversal** of the binary tree_.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/01/31/1236_example_1.PNG)

```
Input: root = [3,9,20,null,null,15,7]
Output: [[9],[3,15],[20],[7]]
Explanation: Without loss of generality, we can assume the root node is at position (0, 0):
The node with value 9 occurs at position (-1, -1).
The nodes with values 3 and 15 occur at positions (0, 0) and (0, -2).
The node with value 20 occurs at position (1, -1).
The node with value 7 occurs at position (2, -2).
```

**Example 2:**![](https://assets.leetcode.com/uploads/2019/01/31/tree2.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation: The node with value 5 and the node with value 6 have the same position according to the given scheme.
However, in the report [1,5,6], the node with value 5 comes first since 5 is smaller than 6.
```

**Constraints:**

* The number of nodes in the tree is in the range `[1, 1000]`.
* `0 <= Node.val <= 1000`

这题，我一看，好像以前见过，然后翻了翻笔记发现，不就是[314 Binary Tree Vertical Order Traversal](314-binary-tree-vertical-order-traversal.md)嘛。然后屁颠屁颠地抄了答案。后来有个test case老是过不了。后来认真审题才发现，这题对list里面数字的排序也有要求，先按深度排，高的排前面，再安值排。高度相等的，值小排前面。这....不就是314多加一个Comparator的事情吗？这题很迷。

```java
public List<List<Integer>> verticalTraversal(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) {
        return result;
    }

    Map<Integer, List<Pair>> colToNode = new TreeMap<>();
    Queue<Pair> queue = new LinkedList<>();
    queue.offer(new Pair(0, 0, root));
    while (!queue.isEmpty()) {
        Pair cur = queue.poll();
        int col = cur.col;
        int row = cur.row;
        TreeNode node = cur.node;

        List<Pair> tmp = colToNode.getOrDefault(col, new ArrayList<>());
        tmp.add(cur);
        colToNode.put(col, tmp);

        if (node.left != null) {
            queue.offer(new Pair(col - 1, row - 1, node.left));
        }

        if (node.right != null) {
            queue.offer(new Pair(col + 1, row - 1, node.right));
        }
    }

    colToNode.keySet();
    for (Map.Entry<Integer, List<Pair>> entry : colToNode.entrySet()) {
        List<Pair> tmp = entry.getValue();
        Collections.sort(tmp, (e1, e2) -> 
                         {
                             if (e2.row == e1.row) {
                                 return e1.node.val - e2.node.val;
                             } else {
                                 return e2.row - e1.row;
                             }

                          });
        List<Integer> values = new ArrayList<>();
        for (Pair p : tmp) {
            values.add(p.node.val);
        }
        result.add(values);
    }

    return result;
}

class Pair {
    int col;
    int row;
    TreeNode node;
    
    public Pair(int c, int r, TreeNode n) {
        col = c;
        row = r;
        node = n;
    }    
}
```
