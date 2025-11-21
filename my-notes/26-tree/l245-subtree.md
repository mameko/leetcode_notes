# L245 Subtree

You have two every large binary trees:`T1`, with millions of nodes, and`T2`, with hundreds of nodes. Create an algorithm to decide if`T2`is a subtree of`T1`.

## Notice

A tree T2 is a subtree of T1 if there exists a node n in T1 such that the subtree of n is identical to T2. That is, if you cut off the tree at node n, the two trees would be identical.

**Example**

T2 is a subtree of T1 in the following case:

```
       1                3
      / \              / 
T1 = 2   3      T2 =  4
        /
       4
```

T2 isn't a subtree of T1 in the following case:

```
       1               3
      / \               \
T1 = 2   3       T2 =    4
        /
       4
```

这题来自ctci 4.10。如果两棵树都不大的话，我们可以通过判断T2的preorder+null是否T1的perorder+null序列的子序列来判断是否subtree。会花费O(n + m)的空间和时间。但如果T1非常大的话，我们可以先从T1里找T2root的值，找到了再判断是否subtree。这个方法会花O(logn + logm)的空间，平均时间是O(n + km)，k是T1里T2root值的个数。worse case时间会变成O(nm)。

```java
public boolean isSubtree(TreeNode T1, TreeNode T2) { // ---27ms
    if (T1 == null && T2 != null) {
        return false;
    } else if (T2 == null) {
        return true;
    }

    boolean found = false;
    if (T1.val == T2.val) {
        found = same(T1, T2);
    } 

    if (found) {
        return true;
    } else {
        return isSubtree(T1.left, T2) || isSubtree(T1.right, T2);
    }
}

private boolean same(TreeNode n1, TreeNode n2) {
    if (n1 == null && n2 == null) {
        return true;
    } else if (n1 == null && n2 != null) {
        return false;
    } else if (n1 != null && n2 == null) {
        return false;
    }

    if (n1.val != n2.val) {
        return false;
    }

    return same(n1.left, n2.left) && same(n1.right, n2.right);
}

//-------用遍历好像比递归快，16ms
public boolean isSubtree(TreeNode s, TreeNode t) {
    if (s == null && t != null) {
        return false;
    } else if (t == null) {
        return true;
    }

    Stack<TreeNode> stack = new Stack<>();
    stack.push(s);
    while (!stack.isEmpty()) {
        TreeNode cur = stack.pop();
        if (cur.val == t.val) {
            if (isSame(cur, t)) {
                return true;
            }
        }

        if (cur.right != null) {
            stack.push(cur.right);
        }

        if (cur.left != null) {
            stack.push(cur.left);
        }
    }

    return false;
}

private boolean isSame(TreeNode s, TreeNode t) {
    if (s == null && t == null) {
        return true;
    } else if (s == null || t == null) {
        return false;
    }

    if (s.val != t.val) {
        return false;
    }

    boolean left = isSame(s.left, t.left);
    boolean right = isSame(s.right, t.right);

    return left && right;
}
```
