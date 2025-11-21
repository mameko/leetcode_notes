# 230 Kth Smallest Element in a BST

Given a binary search tree, write a function`kth Smallest`to find the **k**th smallest element in it.

**Note:**\
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Follow up:**\
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kth Smallest routine?

**Hint:**

1. Try to utilize the property of a BST.
2. What if you could modify the BST node's structure?
3. The optimal runtime complexity is O(height of BST).

解法一：脑残的我只能想到中序遍历然后一个一个数O(n)。解法二是来自leetcode discuss里的二分做法O（height），感觉有点像4 Median of Two Sorted Arrays那题。解法一：T:O(n)，S:O(n)，解法二：T:O(height)平均，O(N^2) worst，S:O(height)，空间是递归的深度

```java
public int kthSmallest(TreeNode root, int k) {
    if (root == null) {
        return Integer.MAX_VALUE;
    }

    Stack<TreeNode> s = new Stack<>();
    TreeNode cur = root;
    int cnt = 0;

    while (!s.isEmpty() || cur != null) {
        while (cur != null) {
            s.push(cur);
            cur = cur.left;
        }

        cur = s.pop();
        cnt++;
        if (cnt == k) {
            return cur.val;
        }

        cur = cur.right;
    }

    return Integer.MAX_VALUE;
}
```

```java
public int kthSmallest(TreeNode root, int k) {
    int count = countNodes(root.left);
    if (k <= count) {
        return kthSmallest(root.left, k);
    } else if (k > count + 1) {
        return kthSmallest(root.right, k-1-count); // 1 is counted as current node
    }

    return root.val;
}

public int countNodes(TreeNode n) {
    if (n == null) return 0;

    return 1 + countNodes(n.left) + countNodes(n.right);
}
```
