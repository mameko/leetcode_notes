# L246 Binary Tree Path Sum II

Your are given a binary tree in which each node contains a value. Design an algorithm to get all paths which sum to a given value. The path does not need to start or end at the root or a leaf, but it must go in a straight line down.

**Example**

Given a binary tree:

```
    1
   / \
  2   3
 /   /
4   2
```

for target =`6`, return

```
[
  [2, 4],
  [1, 3, 2]
]
```

```java
public List<List<Integer>> binaryTreePathSum2(TreeNode root, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    // do pre-order, 
    // sum is for prefix sum from root to leaf
    // tmp is for actual nodes
    ArrayList<Integer> tmp = new ArrayList<>();
    ArrayList<Integer> sum = new ArrayList<>();
    helper(res, tmp, sum, target, root);

    return res;
}

private void helper(List<List<Integer>> res, ArrayList<Integer> tmp, ArrayList<Integer> sum, int target, TreeNode root) {
        // when we reach leaf, we check what we should add to the res
    if (root == null) {
        return;
    }

    // add cur node value to sum & tmp
    tmp.add(root.val);
    sum.add(0);
    for (int i = 0; i < sum.size(); i++) {
        int newSum = sum.get(i) + root.val;
        sum.set(i, newSum);
        // also check if we add up to target yet, if so, add to res
        if (newSum == target) {
            res.add(new ArrayList<>(tmp.subList(i, tmp.size())));
        }
    }

    if (root.left != null) {
        helper(res, tmp, sum, target, root.left);
        tmp.remove(tmp.size() - 1);
        sum.remove(sum.size() - 1);
        for (int i = 0; i < sum.size(); i++) {
            int newSum = sum.get(i) - root.left.val;
            sum.set(i, newSum);
        }
    }

    if (root.right != null) {            
        helper(res, tmp, sum, target, root.right);
        sum.remove(sum.size() - 1);
        tmp.remove(tmp.size() - 1); 
        for (int i = 0; i < sum.size(); i++) {
            int newSum = sum.get(i) - root.right.val;
            sum.set(i, newSum);
        }
    }
}
```
