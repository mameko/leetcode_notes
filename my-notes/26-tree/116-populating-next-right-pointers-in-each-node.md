# 116 Populating Next Right Pointers in Each Node

Given a binary tree

```
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to`NULL`.

Initially, all next pointers are set to`NULL`.

**Note:**

* You may only use constant extra space.
* You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

For example,\
Given the following perfect binary tree,

```
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
```

After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```

这题如果没要求是O(1)的话，可以用queue来层遍历着做。

```java
public void connect(TreeLinkNode root) {
    if (root == null) {
        return;
    }

    // use first to denote the 1st node at each level
    TreeLinkNode first = root;
    // use cur to move right and add next pointers
    TreeLinkNode cur = null;

    // because we are modifying next level, so need to make sure we have next level
    while (first.left != null) {
        // set cur to starting point
        cur = first;
        // if there are right nodes exist
        while (cur != null) {
            // connect next level's left to right 
            cur.left.next = cur.right;
            // if not at the end of this level
            if (cur.next != null) {
                // add the next pointer between 2 sub trees                    
                cur.right.next = cur.next.left;
            }
            // move cur pointer to next node
            cur = cur.next;
        }

        first = first.left;
    }
}

// 2年后写的版本
public void connect(TreeLinkNode root) {
    if (root == null) {
        return;
    }

    TreeLinkNode cur = root;
    TreeLinkNode nextLevel = cur.left;
    while (nextLevel != null) {
        while (cur != null) {
            cur.left.next = cur.right;
            if (cur.next != null) {                
                cur.right.next = cur.next.left;                
            }
            cur = cur.next;
        }
        cur = nextLevel;
        nextLevel = cur.left;
    }
}
```
