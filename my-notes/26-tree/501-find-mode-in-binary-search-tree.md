# 501 Find Mode in Binary Search Tree

Given a binary search tree (BST) with duplicates, find all the\[mode(s)]\([https://en.wikipedia.org/wiki/Mode\_(statistics))(the](https://en.wikipedia.org/wiki/Mode_\(statistics\)\)\(the) most frequently occurred element) in the given BST.

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
* The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
* Both the left and right subtrees must also be binary search trees.

For example:\
Given BST`[1,null,2,2]`,

```
   1
    \
     2
    /
   2
```

return`[2]`.

**Note:**&#x49;f a tree has more than one mode, you can return them in any order.

**Follow up:**&#x43;ould you do that without using any extra space? (Assume that the implicit stack space incurred due to recursion does not count).

这里我自己只想到用hashmap来数数目，然后返回这种方法。看了一下discuss里的O(1)感觉太复杂...所以不写了。

```java
public int[] findMode(TreeNode root) {
    if (root == null) {
        return new int[0];
    }

    HashMap<Integer, Integer> map = new HashMap<>();
    helper(root, map);

    int max = 0;
    for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
        max = Math.max(entry.getValue(), max);
    }

    ArrayList<Integer> tmp = new ArrayList<>();
    for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
        if (entry.getValue() == max) {
            tmp.add(entry.getKey());
        }
    }

    int i = 0;
    int[] res = new int[tmp.size()];
    for (Integer in : tmp) {
        res[i++] = in;
    }

    return res;
}

private void helper(TreeNode root, HashMap<Integer, Integer> map) {
    if (root == null) {
        return;
    }

    if (map.containsKey(root.val)) {
        map.put(root.val, map.get(root.val) + 1);
    } else {
        map.put(root.val, 1);
    }

    helper(root.left, map);
    helper(root.right, map);
}
```
