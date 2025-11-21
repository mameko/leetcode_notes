# 257 Binary Tree Paths

Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:

```
   1
 /   \
2     3
 \
  5
```

All root-to-leaf paths are:

```
["1->2->5", "1->3"]
```

其实如果不用"->"会简单好多，因为加了->很难backtrack，不知道是-2还是-3，因为还得考虑负数。

```java
public List<String> binaryTreePaths(TreeNode root) {
    List<String> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    ArrayList<Integer> tmp = new ArrayList<Integer>();
    ArrayList<ArrayList<Integer>> midRes = new ArrayList<>();
    dfs(midRes, tmp, root);

    for (ArrayList<Integer> list : midRes) {
        StringBuilder withArrow = new StringBuilder();
        for (Integer in : list) {
            withArrow.append(in);
            withArrow.append("->");
        }
        withArrow.setLength(withArrow.length() - 2);
        res.add(withArrow.toString());
    }

    return res;
}

private void dfs(ArrayList<ArrayList<Integer>> res, ArrayList<Integer> tmp, TreeNode root) {
    if (root == null) {
        return;
    }

    tmp.add(root.val);
    if (root.left == null && root.right == null) {
        res.add(new ArrayList<>(tmp));
        return;
    }


    if (root.left != null) {
        dfs(res, tmp, root.left);
        tmp.remove(tmp.size() - 1);
    }

    if (root.right != null) {
        dfs(res, tmp, root.right);
        tmp.remove(tmp.size() - 1);
    }
}
```
