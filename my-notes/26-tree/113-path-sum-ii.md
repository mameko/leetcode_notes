# 113 Path Sum II

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:

Given the below binary tree and`sum = 22`,

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

return

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```

果然是自己写的code，跟前一题的结构基本一样。

```java
public List<List<Integer>> pathSum(TreeNode root, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (root == null) {
        return res;
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    helper(root, target, 0, res, tmp);
    return res;
}

private void helper(TreeNode root, int target, int currentSum, List<List<Integer>> res, ArrayList<Integer> tmp) {
    if (root == null) {
        return;
    }

    currentSum += root.val;
    tmp.add(root.val);        

    if (root.left == null && root.right == null) {
        if (target == currentSum) {
            res.add(new ArrayList<>(tmp));
            return;
        }
    }

    if (root.left != null) {
        helper(root.left, target, currentSum, res, tmp);
        tmp.remove(tmp.size() - 1);
    }

    if (root.right != null) {
        helper(root.right, target, currentSum, res, tmp);
        tmp.remove(tmp.size() - 1);
    }
}

// 参照大神做法写的：
public List<List<Integer>> pathSum(TreeNode root, int sum) {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    if (root == null) {
        return res;
    }

    List<Integer> tmp = new ArrayList<>();
    helper(root, res, tmp, sum);
    return res;
}

private void helper(TreeNode root, List<List<Integer>> res, List<Integer> tmp, int sum) {
    if (root == null) {
        return;
    }

    tmp.add(root.val);

    if (root.left == null && root.right == null && root.val == sum) {
        res.add(new ArrayList<Integer>(tmp));
        return;
    }

    if (root.left != null) {
        helper(root.left, res, tmp, sum - root.val);
        tmp.remove(tmp.size() - 1);
    }

    if (root.right != null) {
        helper(root.right, res, tmp, sum - root.val);
        tmp.remove(tmp.size() - 1);
    }
}
```

其他大神的code

```java
public List<List<Integer>> pathSum(TreeNode root, int sum){
    List<List<Integer>> result  = new LinkedList<List<Integer>>();
    List<Integer> currentResult  = new LinkedList<Integer>();
    pathSum(root,sum,currentResult,result);
    return result;
}

public void pathSum(TreeNode root, int sum, List<Integer> currentResult,
        List<List<Integer>> result) {

    if (root == null)
        return;
    currentResult.add(new Integer(root.val));
    if (root.left == null && root.right == null && sum == root.val) {
        result.add(new LinkedList(currentResult));
        currentResult.remove(currentResult.size() - 1);//don't forget to remove the last integer
        return;
    } else {
        pathSum(root.left, sum - root.val, currentResult, result);
        pathSum(root.right, sum - root.val, currentResult, result);
    }
    currentResult.remove(currentResult.size() - 1);
}
```
