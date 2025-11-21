# 337 House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
     3
    / \
   2   3
    \   \ 
     3   1
```

Maximum amount of money the thief can rob = 3 + 3 + 1 = **7**.

**Example 2:**

```
     3
    / \
   4   5
  / \   \ 
 1   3   1
```

Maximum amount of money the thief can rob = 4 + 5 = **9** .

这题主要是得用一个result type把这层的结果和之前一层的结果记录下来。然后最后返回的时候，返回两者之间的最大值。中间更新的时候要注意，不跳过的时候，取左右两边的max加起来。跳过的时候就直接把root的value和孙子节点里的max加起来。T:O(n)要遍历所有节点，从下向上。S:O(height)，递归栈的深度=树的高度。

```java
public int houseRobber3(TreeNode root) {
    if (root == null) {
        return 0;
    }

    ResultType res = dfs(root);
    return Math.max(res.child, res.meAndGrandChild);
}

private ResultType dfs(TreeNode root) {
    if (root == null) {
        return new ResultType(0, 0);
    }

    ResultType leftRes = dfs(root.left);
    ResultType rightRes = dfs(root.right);

    int child = Math.max(leftRes.child, leftRes.meAndGrandChild) + Math.max(rightRes.child, rightRes.meAndGrandChild);
    int meAndGrandChild = root.val + leftRes.child + rightRes.child;

    return new ResultType(child, meAndGrandChild);
}

class ResultType {
    int child ;
    int meAndGrandChild;

    public ResultType(int ns, int s) {
        child = ns;
        meAndGrandChild = s;
    }
}
```
