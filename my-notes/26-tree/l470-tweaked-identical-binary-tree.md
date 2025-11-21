# L470 Tweaked identical Binary Tree

Check two given binary trees are identical or not. Assuming any number of tweaks are allowed. A tweak is defined as a swap of the children of one node in the tree.

```
Notice:
There is no two nodes with the same value in the tree.
```

Example

```
    1             1
   / \           / \
  2   3   and   3   2
 /                   \
4                     4
```

are identical.

```
    1             1
   / \           / \
  2   3   and   3   2
 /             /
4             4
```

are not identical.

```java
public boolean isTweakedIdentical(TreeNode a, TreeNode b) {
    if (a == null && b == null) {
        return true;
    } else if (a == null || b == null) {
        return false;
    }

    if (a.val != b.val) {
        return false;
    }

    boolean LL = isTweakedIdentical(a.left, b.left);
    boolean RR = isTweakedIdentical(a.right, b.right);
    if (LL && RR) {
        return true;
    }

    boolean LR = isTweakedIdentical(a.left, b.right);
    boolean RL = isTweakedIdentical(a.right, b.left);
    if (LR && RL) {
        return true;
    }

    return false;
}
```
