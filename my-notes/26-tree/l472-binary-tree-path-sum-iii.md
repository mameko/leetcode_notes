# L472 Binary Tree Path Sum III

Give a binary tree, and a target number, find all path that the sum of nodes equal to target, the path could be start and end at any node in the tree.

**Example**

Given binary tree:

```
    1
   / \
  2   3
 /
4
```

and target =`6`. Return :

```
[
  [2, 4],
  [2, 1, 3],
  [3, 1, 2],
  [4, 2]
]
```

因为有parent pointer，这题其实是图的bfs，dfs的题。先bfs遍历，然后每个点再dfs找path sum == target。O(n^2)

```java
public List<List<Integer>> binaryTreePathSum3(ParentTreeNode root, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    // bfs to traverse all node, then do dfs for each to find sum == target
    Queue<ParentTreeNode> queue = new LinkedList<>();
    queue.offer(root);

    while (!queue.isEmpty()) {
        ParentTreeNode cur = queue.poll();
        // do dfs on each node
        Set<ParentTreeNode> visited = new HashSet<ParentTreeNode>();
        dfsHelper(0, target, cur, res, new ArrayList<Integer>(), visited);

        if (cur.left != null) {
            queue.offer(cur.left);
        }

        if (cur.right != null){
            queue.offer(cur.right);
        }

    }

    return res;
}

private void dfsHelper(int sum, int target, ParentTreeNode root, List<List<Integer>> res, ArrayList<Integer> tmp, Set<ParentTreeNode> visited) {

    sum += root.val;
    visited.add(root);
    tmp.add(root.val);

    if (sum == target) {
        res.add(new ArrayList<>(tmp));
    }

    if (root.parent != null && !visited.contains(root.parent)) {
        dfsHelper(sum, target, root.parent, res, tmp, visited);    
        tmp.remove(tmp.size() - 1);
    }

    if (root.left != null && !visited.contains(root.left)) {
        dfsHelper(sum, target, root.left, res, tmp, visited);    
        tmp.remove(tmp.size() - 1);
    }

    if (root.right != null && !visited.contains(root.right)) {
        dfsHelper(sum, target, root.right, res, tmp, visited);    
        tmp.remove(tmp.size() - 1);
    }

    visited.remove(root);
}
```
