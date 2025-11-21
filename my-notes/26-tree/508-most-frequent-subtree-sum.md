# 508 Most Frequent Subtree Sum

Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.

**Examples 1**\
Input:

```
  5
 /  \
2   -3
```

return \[2, -3, 4], since all the values happen only once, return all of them in any order.

**Examples 2**\
Input:

```
  5
 /  \
2   -5
```

return \[2], since 2 happens twice, however -5 only occur once.

**Note:**&#x59;ou may assume the sum of values in any subtree is in the range of 32-bit signed integer.

这题感觉很正规就是分步骤解决问题，有空可以想想follow up会怎么变。

```java
int max = 0; // global max
public int[] findFrequentTreeSum(TreeNode root) {
    if (root == null) {
        return new int[0];
    }

    // <sum, frequency>
    HashMap<Integer, Integer> sumFrequency = new HashMap<>();

    helper(root, sumFrequency);

    // 1 find the most frequent sum (max), use hashmap to store frequency
    for (Map.Entry<Integer, Integer> entry : sumFrequency.entrySet()) {
        max = Math.max(max, entry.getValue());
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    // 2 return all the values that has max frequency
    for (Map.Entry<Integer, Integer> entry : sumFrequency.entrySet()) {
        if (entry.getValue() == max) {
            tmp.add(entry.getKey());
        }
    }

    int i = 0;
    int[] res = new int[tmp.size()];
    for (Integer elem : tmp) {
        res[i++] = elem;
    }

    return res;
}

private int helper(TreeNode root, HashMap<Integer, Integer> hm) {
    if (root == null) {
        return 0;
    }

    int left = helper(root.left, hm);
    int right = helper(root.right, hm);

    int curSum = root.val + left + right;
    if (hm.containsKey(curSum)) {
        hm.put(curSum, hm.get(curSum) + 1);
    } else {
        hm.put(curSum, 1);
    }

    return curSum;
}
```
