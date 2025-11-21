# L474 Lowest Common Ancestor II

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node has an extra attribute`parent`which point to the father of itself. The root's parent is null.

**Example**

For the following binary tree:

```
  4
 / \
3   7
   / \
  5   6
```

LCA(3, 5) =`4`

LCA(5, 6) =`7`

LCA(6, 7) =`7`

从点开始向上遍历，把从两个点到根的路径分别存起来。然后找intersection，相交的那个点就是LCA。

```java
public ParentTreeNode lowestCommonAncestorII(ParentTreeNode root,
                                             ParentTreeNode A,
                                             ParentTreeNode B) {
    if (root == null) {
        return null;
    }

    ArrayList<ParentTreeNode> AList = new ArrayList<>();
    while (A != null) {
        AList.add(A);
        A = A.parent;
    }

    ArrayList<ParentTreeNode> BList = new ArrayList<>();
    while (B != null) {
        BList.add(B);
        B = B.parent;
    }

    int Acur = AList.size() - 1;
    int Bcur = BList.size() - 1;
    ParentTreeNode lca = null;
    while (Acur >= 0 && Bcur >= 0) {
        if (AList.get(Acur) != BList.get(Bcur)) {
            break;
        }
        lca = AList.get(Acur);
        Acur--;
        Bcur--;
    }

    return lca;
}
```
