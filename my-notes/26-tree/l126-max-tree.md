# L126 Max Tree

Given an integer array with no duplicates. A max tree building on this array is defined as follow:

* The root is the maximum number in the array
* The left subtree and right subtree are the max trees of the subarray divided by the root number.

Construct the max tree by the given array.

**Example**

Given`[2, 5, 6, 0, 3, 1]`, the max tree constructed by this array is:

```
    6
   / \
  5   3
 /   / \
2   0   1
```

[**Challenge**](http://www.lintcode.com/en/problem/max-tree/#challenge)

O(n) time and memory.

```java
public TreeNode maxTree(int[] A) {
    if (A == null || A.length == 0) {
        return null;
    }

    Stack<TreeNode> monStack = new Stack<>();
    for (int i = 0; i < A.length; i++) {
        TreeNode cur = new TreeNode(A[i]);
        while (!monStack.isEmpty() && cur.val > monStack.peek().val) {
            cur.left = monStack.pop();
        }

        if (!monStack.isEmpty()) {
            monStack.peek().right = cur;
        }

        monStack.push(cur);
    }

    TreeNode res = null;
    while (!monStack.isEmpty()) {
        res = monStack.pop();
    }

    return res;
}
```
