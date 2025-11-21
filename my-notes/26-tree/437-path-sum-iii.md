# 437 Path Sum III

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example:**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1


Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

看了[leetcode discuss](https://discuss.leetcode.com/topic/64526/17-ms-o-n-java-prefix-sum-method)才知道这题原来可以用2 sum的思想，通过hashmap把复杂度降到O(n)

先上非O(n)的自己写的方法：

```java
int cnt = 0;
public int pathSum(TreeNode root, int target) {
    if (root == null) {
        return cnt;
    }

    // do pre-order, 
    // sum is for prefix sum from root to leaf
    ArrayList<Integer> sum = new ArrayList<>();
    helper(sum, target, root);

    return cnt;
}

private void helper(ArrayList<Integer> sum, int target, TreeNode root) {
    // when we reach leaf, we check what we should add to the res
    if (root == null) {
        return;
    }

    // add cur node value to sum
    sum.add(0);
    for (int i = 0; i < sum.size(); i++) {
        int newSum = sum.get(i) + root.val;
        sum.set(i, newSum);
        // also check if we add up to target yet, if so, add to res
        if (newSum == target) {
            cnt++;
        }
    }

    if (root.left != null) {
        helper(sum, target, root.left);
        sum.remove(sum.size() - 1);
        for (int i = 0; i < sum.size(); i++) {
            int newSum = sum.get(i) - root.left.val;
            sum.set(i, newSum);
        }
    }

    if (root.right != null) {            
        helper(sum, target, root.right);
        sum.remove(sum.size() - 1);
        for (int i = 0; i < sum.size(); i++) {
            int newSum = sum.get(i) - root.right.val;
            sum.set(i, newSum);
        }
    }
}
```

大神初版：

```java
public int pathSum(TreeNode root, int sum) {
    HashMap<Integer, Integer> preSum = new HashMap();
    preSum.put(0,1);
    helper(root, 0, sum, preSum);
    return count;
}
int count = 0;
public void helper(TreeNode root, int currSum, int target, HashMap<Integer, Integer> preSum) {
    if (root == null) {
        return;
    }

    currSum += root.val;

    if (preSum.containsKey(currSum - target)) {
        count += preSum.get(currSum - target);
    }

    if (!preSum.containsKey(currSum)) {
        preSum.put(currSum, 1);
    } else {
        preSum.put(currSum, preSum.get(currSum)+1);
    }

    helper(root.left, currSum, target, preSum);
    helper(root.right, currSum, target, preSum);
    preSum.put(currSum, preSum.get(sum) - 1);
}
```

终极版：

```java
public int pathSum(TreeNode root, int sum) {
    HashMap<Integer, Integer> preSum = new HashMap();
    preSum.put(0,1);
    return helper(root, 0, sum, preSum);
}

public int helper(TreeNode root, int currSum, int target, HashMap<Integer, Integer> preSum) {
    if (root == null) {
        return 0;
    }

    currSum += root.val;
    int res = preSum.getOrDefault(currSum - target, 0);
    preSum.put(currSum, preSum.getOrDefault(currSum, 0) + 1);

    res += helper(root.left, currSum, target, preSum) + helper(root.right, currSum, target, preSum);
    preSum.put(currSum, preSum.get(currSum) - 1);
    return res;
}
```
