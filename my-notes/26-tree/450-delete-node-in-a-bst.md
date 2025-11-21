# 450 Delete Node in a BST

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.

**Note:**&#x54;ime complexity should be O(height of tree).

**Example:**

```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

    5
   / \
  4   6
 /     \
2       7

Another valid answer is [5,2,6,null,4,null,7].

    5
   / \
  2   6
   \   \
    4   7
```

```java
public TreeNode removeNode(TreeNode root, int value) {

    if (root == null) {
        return root;
    }

    if (value < root.val) {
        root.left = removeNode(root.left, value);
    } else if (value > root.val) {
        root.right = removeNode(root.right, value);
    } else {
        // the node need to be deleted may be 3 types of node
        // case 1 : leave node, just delete it
        if (root.left == null && root.right == null) {
            return null;
            // case 2 : has single child, replace node with it's child
        } else if (root.left == null && root.right != null) {
            return root.right;
        } else if (root.left != null && root.right == null) {
            return root.left;
        } else {
            // case 3 : have 2 children, need to replace with inorder succesor then recursively delete child
            TreeNode suc = findSuc(root, value);
            root.val = suc.val;
            // because root has both child, so successor must in the right subtree
            root.right = removeNode(root.right, suc.val);
        }
    }
    return root;
}

private TreeNode findSuc(TreeNode root, int target) {
    TreeNode suc = null;
    while (root != null && root.val != target) {
        if (root.val > target) {
            suc = root;
            root = root.left;
        } else {
            root = root.right;
        }
    }

    if (root == null) {// target is not in the tree
        return null;
    }

    if (root.right == null) {// if doesn't have right subtree, return successor
        return suc;
    }

    root = root.right;// suc is left most element in right subtree
    while (root.left != null) {
        root = root.left;
    }
    return root;
}
```
